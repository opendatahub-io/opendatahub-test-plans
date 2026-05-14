---
feature: maas_multi_tenancy
source_key: RHAIRFE-1487
version: 1.0.0
status: Draft
author: QE
components: []
additional_docs: []
last_updated: '2026-05-05'
source_type: null
reviewers: []
---
# MaaS Multi-Tenancy Test Plan
**QE – Per-Tenant Gateway and Identity Isolation Testing**

**Strategy**: [RHAIRFE-1487](https://redhat.atlassian.net/browse/RHAIRFE-1487)
**Architecture (draft)**: MaaS Multi-Tenancy Architecture — RHAIRFE-1487

---

## 1. Executive Summary

### 1.1 Purpose

This test plan validates the operator-managed multi-tenancy capability for MaaS (Models as a Service) in RHOAI. The feature enables platform administrators to provision isolated organizational tenants via a Tenant CR (or ModelsAsService-like successor CR), where the operator automatically creates the tenant's namespace, dedicated Gateway, MaaSAuthPolicy, MaaSSubscription, identity configuration, and Kuadrant-generated rate-limit policies — and cleans them up on deletion.

Multi-tenancy is a critical requirement from AI Service Provider customers and enterprise deployments with multiple departments. Testing must verify that tenant isolation guarantees are maintained across six per-tenant surfaces identified in the architecture: Gateway (ingress/hostname/TLS), MaaSSubscription (model access and limits), MaaSAuthPolicy (auth rules and policy binding), generated Kuadrant policies (AuthPolicy/rate limits), tenant-scoped identity configuration (issuers/clients/JWKS), and usage/billing signals. A shared maas-controller and MaaSModelRef catalog serve all tenants, with access gated per tenant via subscriptions, auth policies, and grants. The test plan covers the gateway-per-tenant deployment pattern (aligned with the PoC) while remaining architecture-agnostic where possible.

### 1.2 Scope

#### In Scope

- Tenant provisioning via Tenant CR (or successor ModelsAsService-like CR) — automated creation of namespace, Gateway, MaaSAuthPolicy, MaaSSubscription, identity configuration, and Kuadrant policies
- Tenant identity isolation — cross-tenant access prevention for models, API keys, subscriptions, and usage data
- Long-lived API key support — generation, validation, and tenant-scoped authentication (months/years lifetime)
- Bring Your Own IDP (BYOIDP) — external identity provider configuration per tenant (issuers, clients, JWKS, secrets)
- Shared foundation model access grants — platform admin granting specific tenants access to shared MaaSModelRef resources
- Per-tenant usage metrics isolation — token counts, request rates, showback data accuracy, billing signal partitioning
- Tenant CR deletion and automated resource cleanup — namespace, Gateway, MaaSSubscription, MaaSAuthPolicy, Kuadrant policies, identity configuration
- Per-tenant rate limits and quota enforcement via Kuadrant RateLimitPolicy
- Gateway-per-tenant deployment pattern (Pattern 1, aligned with PoC and architecture doc)
- Negative testing: cross-tenant token reuse, model/key enumeration, subscription visibility across tenants (per ADR section 6.5)

#### Out of Scope (Other Teams)

- Hosted Control Planes (HCP) deployment pattern — depends on HCP availability in RHOAI; deferred until confirmed
- External billing system implementation — only integration hooks are in scope
- Foundation model training or fine-tuning per tenant
- MaaS single-tenant functionality (covered by existing test suites)
- RHAISTRAT-1120 (MaaS External OIDC Support) — dependency; tested by its own team
- Operator installation and OLM lifecycle (covered by operator team)

### 1.3 Test Objectives

1. Verify that a platform admin can provision a new tenant via a single ModelsAsService-like CR, and the operator automatically creates the tenant namespace, gateway, identity realm, and policies
2. Verify that tenant users cannot discover or access models, API keys, or usage data belonging to other tenants (hard identity and data isolation)
3. Verify that long-lived API keys (months/years lifetime) are supported, correctly scoped to tenant identity, and functional for model inference authentication
4. Verify that a tenant can configure an external identity provider (BYOIDP) and authenticate users through the federated IDP
5. Verify that platform admins can expose shared foundation models to specific tenants via an access grant mechanism, and unauthorized tenants are denied access
6. Verify that per-tenant usage metrics (token counts, request rates) are correctly isolated and accurately attributed for showback
7. Verify that tenant CR deletion triggers automatic cleanup of all tenant-owned resources (namespace, gateway, routes, identity configuration, policies) with no orphaned resources

---

## 2. Test Strategy

### 2.1 Test Levels

- **API Integration Testing** — Validate tenant provisioning APIs, gateway route creation, identity realm configuration, and model access grant endpoints
- **Data Validation Testing** — Verify tenant isolation at the data layer (API keys, usage metrics, model catalog visibility), cross-tenant data leakage prevention, and billing integration data accuracy
- **Functional Testing** — Test tenant lifecycle (create, configure, delete), BYOIDP integration flows, shared model access grants, rate limit and quota enforcement per tenant
- **Performance Testing** — Assess gateway routing overhead with multiple tenants, identity realm lookup latency, concurrent tenant provisioning, and multi-tenant workload scalability
- **Security Testing** — Verify tenant identity isolation, RBAC boundaries between tenants, API key scoping and validation, external IDP authentication flows, and privilege escalation prevention

### 2.2 Test Types

- **Positive Testing** — Valid tenant provisioning workflows, successful BYOIDP configuration, authorized model access within tenant scope, API key lifecycle management
- **Negative Testing** — Unauthorized cross-tenant access attempts, invalid IDP configurations, malformed API keys, tenant resource access after deletion, quota violation scenarios
- **Boundary Testing** — Maximum number of tenants per cluster, concurrent tenant operations, large model catalog filtering per tenant, API key expiration edge cases (months/years lifetime)
- **Regression Testing** — Ensure existing single-tenant MaaS functionality remains intact, verify shared foundation model access patterns continue to work, validate existing rate-limit and quota mechanisms

### 2.3 Test Priorities

- **P0 (Critical)** — Tenant identity isolation (no cross-tenant access to models, API keys, or usage data), automated tenant resource cleanup on CR deletion, shared model access control enforcement
- **P1 (High)** — BYOIDP integration flows, per-tenant rate limits and quotas, long-lived API key generation and validation, usage metrics isolation for billing integration
- **P2 (Medium)** — Tenant-specific model catalog visibility, billing integration hooks, documentation coverage for onboarding and configuration

---

## 3. Test Environment

### 3.1 Test Cluster Configuration

- OpenShift cluster 4.14+
- RHOAI with MaaS operator (from opendatahub-io/models-as-a-service repository)
- Kuadrant operator (for AuthPolicy and RateLimitPolicy generation)
- Gateway API implementation (for per-tenant Gateway resources)
- Identity management system (TBD per open question 6.2: realm-per-tenant vs single IdP with audience/claim discrimination)
- Metrics and monitoring stack for usage tracking (Prometheus, Thanos, or equivalent)
- Database infrastructure (TBD per open question 6.1: shared DB with tenant_id partitioning vs separate DB per tenant vs separate maas-api per tenant)

### 3.2 Test Data Requirements

- Sample Tenant CRs with varying gateway references and OIDC configurations
- MaaSSubscription CRs defining model access and limits per tenant
- MaaSAuthPolicy CRs with tenant-specific authentication rules
- MaaSModelRef CRs representing the shared model catalog
- Access grant configuration for cross-tenant model sharing (format TBD)
- Mock OIDC issuer configurations (JWKS endpoints, client credentials) for BYOIDP testing
- Sample API key sets scoped to different tenant identities
- Usage/billing signal data for metric isolation verification
- Sample Gateway and HTTPRoute configurations per tenant with ingress/hostname/TLS posture
- Test model catalog data with varying visibility scopes per tenant

### 3.3 Test Users

- **Platform admin** — Cluster-admin privileges for provisioning Tenant CRs, managing maas-controller, and configuring shared MaaSModelRef resources
- **Tenant admin** — Namespace-scoped permissions for managing tenant resources (RBAC scope TBD per open question 6.3)
- **Tenant user** — Regular user consuming models via tenant-scoped API keys
- **BYOIDP test user** — Federated identities per tenant with different issuer/audience claims for external IDP testing
- **Service account** — Per-tenant service accounts for API key validation and automation scenarios
- **Cross-tenant attacker** — Users with valid credentials from Tenant A attempting access to Tenant B resources (negative testing)
- **Anonymous user** — For unauthorized access testing

---

## 4. Interfaces Under Test

### 4.1 Tenant CR Operations

| Interface | Method | Purpose | Priority |
|-----------|--------|---------|----------|
| Tenant CR (or successor ModelsAsService-like CR) | CREATE | Provision new tenant — triggers operator to create namespace, Gateway, MaaSAuthPolicy, MaaSSubscription, identity configuration, and Kuadrant policies | P0 |
| Tenant CR | DELETE | Remove tenant — triggers automated cleanup of all tenant-owned resources via finalizers | P0 |
| Tenant CR | UPDATE | Modify tenant configuration (gateway references, OIDC config, quota settings) | P1 |
| Tenant CR | GET/LIST | Retrieve tenant status and configuration | P1 |

### 4.2 Per-Tenant Custom Resources

| Interface | Method | Purpose | Priority |
|-----------|--------|---------|----------|
| MaaSSubscription CR | CREATE | Define which models a tenant can use and under what limits; must be tenant-scoped | P0 |
| MaaSSubscription CR | UPDATE/DELETE | Modify or remove model subscriptions for a tenant | P1 |
| MaaSAuthPolicy CR | CREATE | Define tenant-specific authentication rules, IdP claims, audiences, and policy binding | P0 |
| MaaSAuthPolicy CR | UPDATE/DELETE | Modify or remove tenant auth policies | P1 |
| Kuadrant AuthPolicy (generated) | Verify creation | Confirm gateway-facing auth policy is generated from MaaSAuthPolicy per tenant | P1 |
| Kuadrant RateLimitPolicy (generated) | Verify creation | Confirm gateway-facing rate limit policy is generated per tenant from subscription limits | P1 |

### 4.3 Shared Resources

| Interface | Method | Purpose | Priority |
|-----------|--------|---------|----------|
| MaaSModelRef CR | GET/LIST | Shared model catalog; list/get operations must enforce tenant scope and access grants — "shared" does not mean "visible in every tenant's API responses" | P0 |
| Shared model access grant mechanism | CREATE | Platform admin grants tenant access to a shared foundation model (mechanism TBD — may be CR, API, or RBAC annotation) | P0 |
| Shared model access grant mechanism | DELETE | Revoke a tenant's access to a shared model | P0 |
| Shared model access grant mechanism | GET/LIST | List access grants for a tenant or model | P1 |

### 4.4 Tenant Resource Verification

| Interface | Method | Purpose | Priority |
|-----------|--------|---------|----------|
| Tenant namespace | Verify creation | Confirm namespace is created with correct labels and network policies | P0 |
| Tenant Gateway (Gateway API) | Verify creation | Confirm dedicated gateway with ingress/hostname/TLS posture is provisioned per tenant | P0 |
| Tenant HTTPRoute | Verify creation | Confirm routes are created for tenant model endpoints on the tenant's gateway | P0 |
| Tenant identity configuration | Verify creation | Confirm tenant-scoped issuers, clients, JWKS, and secrets are configured | P0 |

### 4.5 Authentication and Identity

| Interface | Method | Purpose | Priority |
|-----------|--------|---------|----------|
| Tenant API key (via maas-api) | CREATE | Generate long-lived API key scoped to tenant identity | P0 |
| Tenant API key (via maas-api) | VALIDATE | Authenticate model inference request with tenant-scoped API key | P0 |
| BYOIDP configuration | CREATE/UPDATE | Configure external identity provider per tenant (issuer, client, JWKS endpoints) — depends on RHAISTRAT-1120 | P1 |
| BYOIDP authentication | AUTH | Authenticate via federated external IDP with tenant-scoped claims and audiences | P1 |

### 4.6 Metrics, Usage, and Billing

| Interface | Method | Purpose | Priority |
|-----------|--------|---------|----------|
| Per-tenant usage metrics | GET | Retrieve token counts, request rates per tenant; must be partitioned so one tenant cannot read another's consumption | P1 |
| Billing integration hooks | EXPORT | Export per-tenant metering data for cost attribution (format TBD) | P2 |

### 4.7 Isolation Enforcement (Negative Testing)

| Interface | Method | Purpose | Priority |
|-----------|--------|---------|----------|
| Cross-tenant model access | DENY | Tenant A cannot discover or access Tenant B's models or subscriptions | P0 |
| Cross-tenant API key reuse | DENY | API key from Tenant A cannot authenticate on Tenant B's gateway; no cross-tenant token reuse | P0 |
| Cross-tenant metrics/usage access | DENY | Tenant A cannot view Tenant B's usage data or billing signals | P0 |
| Cross-tenant key/model enumeration | DENY | Tenant users cannot enumerate models, keys, or subscriptions belonging to other tenants | P0 |
| Subscription change isolation | DENY | Subscription changes in Tenant A are invisible to Tenant B | P0 |

---

## 5. Test Cases

> **Note**: Test cases have not been generated yet. To be filled later in the process.

**Test Cases Directory**: [test_cases/](test_cases/)
**Complete Test Case Index**: [test_cases/INDEX.md](test_cases/INDEX.md)

### 5.1 Test Case Organization

> **Note**: To be filled later in the process.

| Category | Test Cases | Priority Distribution |
|----------|------------|----------------------|
| | | |

### 5.2 Test Case Naming Convention

Test cases follow the naming pattern: `TC-<CATEGORY>-<NUMBER>`

- **TC-PROV** — Tenant provisioning and CR lifecycle
- **TC-ISO** — Tenant isolation and cross-tenant access prevention
- **TC-AUTH** — Authentication, API keys, and identity management
- **TC-BYOIDP** — Bring Your Own IDP configuration and flows
- **TC-GRANT** — Shared model access grants
- **TC-METRICS** — Per-tenant usage metrics and billing integration
- **TC-CLEANUP** — Tenant deletion and resource cleanup
- **TC-POLICY** — Rate limits, quotas, and policy enforcement
- **TC-E2E** — End-to-end tenant lifecycle scenarios

---

## 6. E2E Test Scenarios

End-to-end scenarios that validate the user journeys defined in the strategy. Each scenario maps to one or more TC-E2E-*.md test cases generated by `/test-plan.create-cases`.

> **Requirement**: At least one E2E scenario MUST be generated for each P0 endpoint in Section 4.
> E2E scenarios will be filled by `/test-plan.create-cases`.

### 6.1 Scenario Summary

> **Note**: E2E scenarios have not been generated yet. To be filled later in the process.

| ID | Scenario | Endpoints Covered | Priority |
|----|----------|-------------------|----------|
| | | | |

### 6.2 E2E Coverage Matrix

> **Note**: To be filled later in the process.

| Endpoint (from Section 4) | E2E Scenarios |
|----------------------------|---------------|
| | |

---

## 7. Non-Functional Requirements

Each category below must be explicitly addressed. If a category does not apply to this feature, state **Not Applicable** with a brief justification.

### 7.1 Disconnected/Air-Gapped

**Not Applicable** — The multi-tenancy feature operates on existing RHOAI infrastructure without introducing new external network dependencies. The strategy does not mention external registry dependencies, operator catalog sources, or runtime image pulls that would require disconnected environment testing. Identity realm integration and gateway provisioning are cluster-internal operations.

### 7.2 Upgrade/Migration

- **Single-tenant to multi-tenant migration** — Existing MaaSSubscription, MaaSAuthPolicy, and usage data must be migrated or assigned to a default tenant when upgrading from single-tenant to multi-tenant model
- **Tenant CR schema stability** — Test upgrades where Tenant CRs created in older RHOAI versions must remain functional after operator upgrade; verify CRD schema changes are backwards compatible
- **Backward compatibility** — Shared maas-controller must reconcile both single-tenant (legacy) and multi-tenant CRs during transition period
- **Identity realm migration** — If identity configuration format changes between versions, test migration of existing tenant realms without forcing re-authentication or API key regeneration
- **Shared model access grants** — Verify upgrade does not break existing cross-namespace model access configurations; test rollback scenarios where tenant namespaces must revert to previous state
- **Multi-tenant to multi-tenant upgrade** — Test operator upgrade path when tenants are already provisioned (no manual intervention required to preserve tenant isolation)
- **Data layer migration** — Migration strategy depends on final data layer decision (shared DB with tenant_id vs separate DB/maas-api per tenant); rollback scenarios must be tested

### 7.3 Performance/Scalability

- **Gateway routing overhead** — Test API request latency with 10, 50, 100+ tenants to identify routing table lookup degradation
- **Identity realm lookup** — Measure authentication time when validating API keys across multiple tenant identity realms
- **Concurrent tenant provisioning** — Test operator behavior when multiple tenant CRs are created simultaneously (namespace creation, gateway provisioning, policy application)
- **Large model catalog filtering** — Assess UI and API response times when filtering shared model catalog visibility per tenant with hundreds of models
- **Usage metrics aggregation** — Test billing integration hook performance when collecting per-tenant token counts and request rates at scale

### 7.4 RBAC/Authorization

- **Tenant-scoped API key validation** — Test that API keys generated for Tenant A cannot authenticate requests to Tenant B's gateway or model endpoints
- **Platform admin vs. tenant user roles** — Verify platform admins can provision/delete tenants but cannot impersonate tenant users; tenant users cannot access platform-level tenant management APIs
- **Shared model access grants** — Test that only explicitly granted tenants can access shared foundation models; unauthorized tenants receive 403 Forbidden responses
- **BYOIDP permission mapping** — Validate that external IDP roles correctly map to tenant-scoped permissions and do not leak into other tenant realms
- **Service account isolation** — Ensure operator service accounts managing tenant resources cannot be used to escalate privileges across tenant boundaries

---

## 8. Risks and Mitigation

| Risk | Impact | Probability | Mitigation |
|------|--------|-------------|------------|
| Dependency on RHAISTRAT-1120 (External OIDC Support) blocks BYOIDP testing until identity realm integration is complete | High | Medium | Coordinate with RHAISTRAT-1120 team for delivery timeline; implement tenant provisioning without BYOIDP first, add OIDC as phase 2 |
| Open question 6.1 (Data layer decision) — Shared DB vs separate DB/maas-api per tenant impacts isolation guarantees, migration complexity, and test coverage | High | High | Escalate architectural decision to design review; defer detailed data migration tests until decision locked; prepare test scenarios for both options |
| Open question 6.2 (Identity realm topology) — Realm-per-tenant vs single IdP design affects BYOIDP integration and token validation paths | Medium | High | Prototype both approaches in test environment; measure latency and operational complexity before committing |
| Cross-tenant enumeration attacks — API keys, model names, or usage data leaked via timing attacks or error messages (per ADR section 6.5 verification requirements) | High | Medium | Implement negative E2E tests early; fuzz endpoints for information disclosure; audit all error responses for tenant identifiers |
| Gateway policy merging conflicts — ADR section 4 notes "merging complications possible" for Kuadrant AuthPolicy/rate limits tied to tenant CRs | Medium | High | Test concurrent tenant provisioning; validate policy merge logic with Kuadrant team; implement conflict detection in maas-controller |
| Open question 6.3 (Namespace topology) — One namespace per tenant vs multiple affects RBAC complexity and resource quotas | Medium | High | Define namespace strategy before E2E test environment setup; document in test plan prerequisites |
| Shared model access grant mechanism is described at high level — unclear if grants are namespace-based, RBAC-based, or custom policy (MaaSModelRef is shared but access must be per-tenant) | Medium | High | Request API spec or design doc for shared model access grant implementation; delay access grant testing until specification is available |
| Automated cleanup on tenant CR deletion may fail partially (orphaned MaaSSubscription, MaaSAuthPolicy, or Kuadrant policies) if operator crashes mid-deletion | Medium | Medium | Add finalizers to Tenant CR; test cascading deletion in CI; verify cleanup via automated post-deletion assertions |
| Long-lived API keys (months/years) introduce key rotation and revocation complexity not addressed in strategy | Medium | Low | Test key lifecycle at accelerated timescales (mock expiration dates); define rotation SOP and test automation for revoked key propagation |
| Per-tenant metric cardinality explosion — high tenant count multiplied by per-tenant labels (open question 6.4) may degrade observability backend | Medium | Medium | Load test metric collection with 100+ tenants; consult observability team for metric cardinality limits |
| Test environment may not support multiple tenants at scale (limited cluster resources for 50+ tenant test) | Medium | Medium | Prioritize functional isolation tests over scale tests; use lightweight mock workloads per tenant; coordinate with infrastructure team for dedicated multi-tenant test cluster |

---

## 9. Test Environment Requirements

### 9.1 Infrastructure

- Single OpenShift cluster for Pattern 1 (gateway-per-tenant) testing
- Single maas-controller deployment managing multiple tenants (expanded RBAC required)
- Per-tenant Gateway instances (dedicated ingress paths, separate hostnames/TLS)
- Kuadrant operator for AuthPolicy and RateLimitPolicy generation
- Database infrastructure — decision pending per ADR open question 6.1 (shared DB with tenant_id partitioning vs separate DB per tenant vs separate maas-api per tenant)
- External OIDC provider infrastructure for BYOIDP testing (depends on RHAISTRAT-1120)
- Shared MaaSModelRef catalog accessible to all tenants with subscription-based gating
- Observability backend capable of tenant-labeled metrics and logs

### 9.2 Configuration

- Tenant CR specifications (gateway references, API/OIDC config per tenant)
- RBAC policies for maas-controller to manage multi-tenant resources across namespaces
- MaaSSubscription CRs defining model access limits per tenant
- MaaSAuthPolicy CRs defining authentication rules per tenant
- Kuadrant AuthPolicy generated from MaaSAuthPolicy CRs
- Kuadrant RateLimitPolicy generated per tenant from subscription limits
- OIDC issuer endpoints, JWKS URLs, client secrets per tenant
- Namespace topology configuration — decision pending per ADR open question 6.3
- Metric/log label configuration for per-tenant showback
- Feature flags for multi-tenancy enablement (if applicable)

### 9.3 Test Tools

- `oc`/`kubectl` for CR management and cluster inspection
- `curl`/`httpie` for API endpoint testing with tenant-scoped API keys
- `jq` for JSON response parsing and validation
- Database query tools (psql/mysql) — pending data layer decision
- OIDC token generators (jwt.io, custom scripts) for creating tenant-scoped tokens
- Prometheus/Grafana for validating per-tenant usage metric isolation
- Log aggregation tools (EFK/Loki) for tenant-partitioned log verification
- CR validation tools (`kubectl dry-run`, `kubeconform`)
- `yq` for YAML manipulation and validation
- Network policy testing tools for tenant isolation verification
- Load testing tools (`k6`, `Locust`, or Apache Bench) for rate-limit and performance testing
- Git for accessing the reference PoC repository (bartoszmajsak/maas-multi-tenancy-poc)

---

## 10. Appendix

### 10.1 Test Case Summary

> **Note**: To be filled later in the process.

| Category | Total | P0 | P1 | P2 |
|----------|-------|----|----|-----|
| | | | | |

### 10.2 Interface Coverage

| Interface | Test Cases | Coverage |
|-----------|------------|----------|
| Tenant CR — CREATE | | |
| Tenant CR — DELETE | | |
| Tenant CR — UPDATE | | |
| Tenant CR — GET/LIST | | |
| MaaSSubscription CR — CREATE | | |
| MaaSSubscription CR — UPDATE/DELETE | | |
| MaaSAuthPolicy CR — CREATE | | |
| MaaSAuthPolicy CR — UPDATE/DELETE | | |
| Kuadrant AuthPolicy — Verify creation | | |
| Kuadrant RateLimitPolicy — Verify creation | | |
| MaaSModelRef CR — GET/LIST | | |
| Shared model access grant — CREATE | | |
| Shared model access grant — DELETE | | |
| Shared model access grant — GET/LIST | | |
| Tenant namespace — Verify creation | | |
| Tenant Gateway — Verify creation | | |
| Tenant HTTPRoute — Verify creation | | |
| Tenant identity configuration — Verify creation | | |
| Tenant API key — CREATE | | |
| Tenant API key — VALIDATE | | |
| BYOIDP configuration — CREATE/UPDATE | | |
| BYOIDP authentication — AUTH | | |
| Per-tenant usage metrics — GET | | |
| Billing integration hooks — EXPORT | | |
| Cross-tenant model access — DENY | | |
| Cross-tenant API key reuse — DENY | | |
| Cross-tenant metrics/usage — DENY | | |
| Cross-tenant key/model enumeration — DENY | | |
| Subscription change isolation — DENY | | |

### 10.3 Document Change Log

| Version | Date | Changes |
|---------|------|---------|
| 1.0.0 | 2026-05-04 | Initial test plan |

---

**End of Test Plan**

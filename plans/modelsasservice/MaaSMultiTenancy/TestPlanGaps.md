---
feature: maas_multi_tenancy
source_key: RHAIRFE-1487
status: Open
gap_count: 21
last_updated: '2026-05-05'
---
# Gaps — MaaS Multi-Tenancy

## Scope & Endpoints

- **Data layer topology decision (ADR open question 6.1)** — Shared DB with tenant_id partitioning vs separate DB/maas-api per tenant is unresolved; impacts API endpoint paths, tenant_id query partitioning strategy, and negative testing scenarios. Would be resolved by: updated ADR with data layer decision and migration plan.
- **Identity realm implementation details (ADR open question 6.2)** — Realm-per-tenant vs single IdP with audience/claim discrimination is unresolved; depends on RHAISTRAT-1120 for external OIDC support. Token issuance endpoints, key rotation/revocation APIs not specified. Would be resolved by: API spec or design doc for RHAISTRAT-1120.
- **Shared model access grant mechanism** — Strategy requires platform admins to grant tenant access to shared MaaSModelRef resources, but ADR does not define the resource type, API, or governance workflow for these grants. Would be resolved by: API spec or design doc.
- **External billing integration hook specification** — Strategy requires "external billing integration hooks for per-tenant metering," but neither document specifies the export format, push vs pull model, or webhook/event schema. Would be resolved by: API spec.
- **Tenant admin RBAC model (ADR open question 6.3)** — "RBAC for platform admins vs tenant admins" is an open question; impacts whether tenants have self-service API access or rely on platform operator intervention. Would be resolved by: design doc or updated ADR.
- **Observability and showback export paths (ADR open question 6.4)** — Metric and log labels for per-tenant showback are unspecified; unclear whether this is Prometheus metrics with tenant labels, a REST API, or a separate telemetry CR per tenant. Would be resolved by: design doc or API spec.
- **CR and namespace topology (ADR open question 6.3)** — One namespace per tenant vs multiple, and how Operator parents child tenant resources, is unresolved; affects resource discovery, RBAC scoping, and cleanup verification. Would be resolved by: updated ADR or design doc.

## Test Strategy & Risks

- **Data layer architecture decision** — Shared DB with tenant_id partitioning vs separate DB/maas-api per tenant is unresolved (ADR open question 6.1); impacts data migration test strategy, isolation verification, and performance benchmarks. Would be resolved by: updated ADR with data layer decision.
- **Identity realm topology** — Realm-per-tenant vs single IdP (ADR open question 6.2); impacts BYOIDP integration testing and token validation test coverage. Would be resolved by: updated ADR with identity architecture decision.
- **Namespace topology** — One namespace per tenant vs multiple (ADR open question 6.3); impacts E2E test environment setup, RBAC test scenarios, and resource quota validation. Would be resolved by: design doc.
- **Negative isolation test scenarios** — ADR section 6.5 lists high-level verification themes (cross-tenant token reuse, model/key enumeration, subscription visibility) but detailed acceptance criteria are not specified. Would be resolved by: feature refinement or security test plan addendum.
- **API key management API surface** — Strategy mentions long-lived API keys but does not specify creation, rotation, revocation, or listing endpoints via maas-api. Would be resolved by: API spec.
- **Tenant CR schema and lifecycle** — "ModelsAsService-like CR" or Tenant CR fields, status conditions, and finalizer behavior are not specified. Would be resolved by: API spec with CRD schema.

## Environment & Infrastructure

- **OpenShift and RHOAI version requirements** — Not specified. Would be resolved by: design doc or feature refinement.
- **MaaS operator version and repository branch/tag** — opendatahub-io/models-as-a-service referenced but specific version not specified. Would be resolved by: implementation plan.
- **Kuadrant operator version** — Not specified. Would be resolved by: design doc.
- **Gateway API implementation choice** — Not specified (Istio, Kong, Envoy Gateway, etc.). Would be resolved by: ADR update.
- **MaaSSubscription and MaaSAuthPolicy CR schemas** — Field definitions not provided. Would be resolved by: API spec or design doc.
- **Tenant cleanup behavior details** — Finalizer logic, grace periods, force-delete policies not detailed. Would be resolved by: design doc.
- **RHAISTRAT-1120 (External OIDC Support) completion status** — Dependency status unknown; blocks BYOIDP testing. Would be resolved by: dependency tracking.
- **PoC branch integration status** — bartoszmajsak/maas-multi-tenancy-poc operator integration branch status not specified. Would be resolved by: implementation plan.

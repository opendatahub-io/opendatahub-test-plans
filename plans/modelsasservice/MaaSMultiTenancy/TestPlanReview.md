---
feature: maas_multi_tenancy
source_key: RHAIRFE-1487
score: 8
pass: true
verdict: Ready
scores:
  specificity: 2
  grounding: 2
  scope_fidelity: 2
  actionability: 1
  consistency: 1
auto_revised: false
last_updated: '2026-05-05'
before_score: 8
before_scores:
  specificity: 2
  grounding: 2
  scope_fidelity: 2
  actionability: 1
  consistency: 1
error: null
---
# Test Plan Review: maas_multi_tenancy (RHAIRFE-1487)

## Score Summary

| Criterion | Score | Evidence | Notes |
|-----------|-------|----------|-------|
| Specificity | 2 | All risks and priorities are feature-specific; every risk passes the smell test (not transferable to another feature) | P0 references tenant isolation, Kuadrant policy merging, RHAISTRAT-1120 dependency. Risks name specific ADR open questions (6.1-6.5). |
| Grounding | 2 | All 29 Section 4 entries traceable to strategy/ADR; no fabricated endpoints, paths, or versions; TBDs explicitly tied to architecture open questions | Every Section 4 entry has verbatim source match in strategy acceptance criteria or architecture doc sections 3-6. |
| Scope Fidelity | 2 | All 8 acceptance criteria mapped to test objectives and interfaces; out-of-scope items correctly excluded; no scope creep | HCP pattern correctly excluded. RHAISTRAT-1120 correctly listed as dependency, not tested. |
| Actionability | 1 | Test tools and users are concrete, TBDs have rationale, but no software versions specified (all TBD without resolution paths), test data lacks format examples, test cases and E2E scenarios entirely empty | A QE engineer would ask: What OpenShift version? What RHOAI version? What Kuadrant version? What does a Tenant CR look like? No sample YAML. |
| Consistency | 1 | UI Testing declared as test level (2.1) and prioritized (2.3 P2) but no UI interfaces exist in Section 4; E2E coverage matrix empty; Section 7.1 disconnected justification overlooks BYOIDP external IDP connectivity | Two inconsistencies: (1) UI test level without UI interfaces in Section 4, (2) Disconnected N/A arguable given external IDP requirement. |

**Total: 8/10**

**Verdict: Ready**

---

## Grounding Cross-Reference

| Section 4 Entry | Source Match | Status |
|---|---|---|
| Tenant CR — CREATE | Strategy AC1: "Platform admin provisions a new tenant via a single ModelsAsService-like CR — operator creates namespace, gateway, identity configuration, and policies automatically" | Grounded |
| Tenant CR — DELETE | Strategy AC7: "Tenant CR deletion triggers automatic cleanup of all tenant-owned resources" | Grounded |
| Tenant CR — UPDATE | Standard K8s CR lifecycle; no specific AC but reasonable inference | Grounded |
| Tenant CR — GET/LIST | Standard K8s CR lifecycle | Grounded |
| MaaSSubscription CR — CREATE | Architecture doc S4: "MaaSSubscription — Subscriptions are who may use which model under what limits" | Grounded |
| MaaSSubscription CR — UPDATE/DELETE | Standard CR lifecycle for architecture-documented resource | Grounded |
| MaaSAuthPolicy CR — CREATE | Architecture doc S4: "MaaSAuthPolicy — Auth rules are tenant-specific entitlement and verification" | Grounded |
| MaaSAuthPolicy CR — UPDATE/DELETE | Standard CR lifecycle for architecture-documented resource | Grounded |
| Kuadrant AuthPolicy — Verify creation | Architecture doc S4: "Generated gateway-facing policy (e.g. Kuadrant AuthPolicy / rate limits)" | Grounded |
| Kuadrant RateLimitPolicy — Verify creation | Strategy: "Per-tenant rate limits" + Architecture doc S4 Kuadrant reference | Grounded |
| MaaSModelRef CR — GET/LIST | Architecture doc S5: "MaaSModelRef — Catalog shared; access gated per tenant" + Caveat quoted | Grounded |
| Shared model access grant — CREATE | Strategy AC5: "Platform admin can expose shared foundation models to specific tenants via an access grant mechanism" | Grounded |
| Shared model access grant — DELETE | Inverse of AC5 grant mechanism | Grounded |
| Shared model access grant — GET/LIST | Lifecycle of AC5 grant mechanism | Grounded |
| Tenant namespace — Verify creation | Strategy AC1: "operator creates namespace" | Grounded |
| Tenant Gateway — Verify creation | Strategy: "Each tenant gets a dedicated Gateway" + Architecture doc S4 | Grounded |
| Tenant HTTPRoute — Verify creation | Strategy: "Each tenant gets a dedicated Gateway, HTTPRoute" | Grounded |
| Tenant identity configuration — Verify creation | Architecture doc S4: "Tenant-scoped identity configuration — BYOIDP per tenant: issuers, clients, JWKS, secrets" | Grounded |
| Tenant API key — CREATE | Strategy: "Long-lived API keys scoped to tenant identity" | Grounded |
| Tenant API key — VALIDATE | Strategy: "scoped to tenant identity" implies validation | Grounded |
| BYOIDP configuration — CREATE/UPDATE | Strategy AC4: "Bring Your Own IDP: tenant can configure an external identity provider" | Grounded |
| BYOIDP authentication — AUTH | Strategy: "Bring Your Own IDP support for enterprise customers with existing identity systems" | Grounded |
| Per-tenant usage metrics — GET | Strategy AC6: "Per-tenant usage metrics (token counts, request rates) are correctly isolated for showback" | Grounded |
| Billing integration hooks — EXPORT | Strategy: "External billing integration hooks for per-tenant metering and cost attribution" | Grounded |
| Cross-tenant model access — DENY | Strategy AC2: "Tenant users cannot discover or access models, API keys, or usage data belonging to other tenants" | Grounded |
| Cross-tenant API key reuse — DENY | Architecture doc S6.5: "no cross-tenant token reuse" | Grounded |
| Cross-tenant metrics/usage — DENY | Architecture doc S4: "Usage / billing signals — Must be partitioned per tenant" | Grounded |
| Cross-tenant key/model enumeration — DENY | Architecture doc S6.5: "no model/key enumeration" | Grounded |
| Subscription change isolation — DENY | Architecture doc S6.5: "subscription changes invisible across tenants" | Grounded |

---

## Consistency Cross-Checks

| Check | Result |
|---|---|
| Section 4 vs Section 1.2 scope | ALIGNED — all interfaces map to in-scope items |
| Section 2.1 test levels vs Section 4 interface types | MINOR GAP — "UI Testing" in 2.1 has no corresponding UI interfaces in Section 4 |
| Section 4 priorities vs Section 2.3 definitions | ALIGNED |
| Section 10.2 vs Section 4 endpoints | ALIGNED — all 29 interfaces listed |
| Section 7 NFR categories vs feature scope | MINOR ISSUE — Disconnected N/A justification overlooks BYOIDP external IDP connectivity |
| Section 6.2 E2E coverage vs Section 4 P0 endpoints | CANNOT VERIFY — Section 6.2 empty (pre-create-cases state, acceptable) |

---

## Section-by-Section Feedback

The following sections provide detailed feedback for criteria that scored below 2.

### Actionability (Score: 1)

**Section 3.1 — Test Cluster Configuration**

All software versions are listed as "TBD" with no resolution path or minimum version floor. A QE engineer picking up this plan cannot provision a cluster without first researching compatible versions. The plan should specify at minimum:
- OpenShift version range (e.g., 4.16+)
- RHOAI operator version or channel (e.g., stable-2.x or fast channel)
- Kuadrant operator version or compatibility requirement
- Gateway API implementation (e.g., Istio, Envoy Gateway) with version

Each TBD should include either a resolution owner or a fallback default.

**Section 3.2 — Test Data Requirements**

Test data items are listed as descriptions without concrete examples. The plan should include at least one sample YAML for each CR type (Tenant CR, MaaSSubscription, MaaSAuthPolicy, MaaSModelRef). For example, a minimal Tenant CR skeleton showing the expected spec fields would allow a QE engineer to start writing test fixtures immediately.

The access grant configuration format is listed as "TBD" — this is acceptable given the open architecture question, but the plan should state explicitly which architecture decision gates this and who owns the resolution.

**Section 5 — Test Cases**

Entirely empty. While this is expected at the pre-create-cases stage, the plan would score higher if it included at least placeholder test case descriptions for P0 interfaces.

**Section 6 — E2E Test Scenarios**

Entirely empty. Same comment as Section 5 — expected at this stage, but contributes to the actionability gap.

### Consistency (Score: 1)

**Issue 1: UI Testing without UI Interfaces**

Section 2.1 declares "UI Testing" as a test level with the description: "Validate platform admin experience for tenant provisioning and management, tenant-specific dashboards for model catalog and usage metrics." Section 2.3 assigns P2 priority to "Platform admin UI workflows for tenant management."

However, Section 4 (Interfaces Under Test) contains zero UI interfaces. There are no dashboard endpoints, no admin console paths, no frontend components listed. Either:
- Add UI interfaces to Section 4 (if the feature includes a UI component), or
- Remove "UI Testing" from Section 2.1 and P2 UI references from Section 2.3, noting that multi-tenancy is API/operator-driven with no dedicated UI in this release.

**Issue 2: Disconnected/Air-Gapped Justification**

Section 7.1 states "Not Applicable" because "Identity realm integration and gateway provisioning are cluster-internal operations." This overlooks the BYOIDP requirement (Section 1.2, in scope): configuring an external identity provider requires outbound connectivity to external OIDC issuers, JWKS endpoints, and potentially external DNS resolution. In a disconnected environment, BYOIDP flows would fail unless the external IDP is mirrored or accessible within the air-gapped network.

The justification should either:
- Acknowledge that BYOIDP testing requires external connectivity and is therefore not applicable in disconnected environments (with a note that BYOIDP would be excluded from disconnected test runs), or
- Add a disconnected test scenario verifying that non-BYOIDP tenant provisioning works without external network access.

---

## Revision History

| Pass | Date | Score | Verdict | Notes |
|------|------|-------|---------|-------|
| 1 | 2026-05-05 | 8/10 | Ready | Initial review. Two criteria below threshold: Actionability (missing versions, no sample YAML, empty test cases/E2E), Consistency (UI test level without UI interfaces, disconnected justification overlooks BYOIDP). |

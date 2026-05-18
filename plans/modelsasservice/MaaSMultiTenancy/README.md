# MaaS Multi-Tenancy — Phase 1: Namespace-Based Tenant Isolation

Operator-managed multi-tenancy for MaaS enabling platform admins to provision isolated organizational tenants via namespace labeling (`maas.opendatahub.io/tenant`), with a single shared Gateway, shared PostgreSQL (`api_keys.tenant` column), and automated MaaSAuthPolicy propagation to Kuadrant.

## Links

- **Jira RFE**: [RHAIRFE-1487](https://redhat.atlassian.net/browse/RHAIRFE-1487)
- **Strategy (updated)**: [RHAISTRAT-1741](https://redhat.atlassian.net/browse/RHAISTRAT-1741) — MaaS Multi-Tenancy Strategy v2
- **Engineering Epic**: [RHOAIENG-62570](https://redhat.atlassian.net/browse/RHOAIENG-62570) — Multi-tenancy Phase 1
- **Architecture (v2)**: [Working Design Doc](https://docs.google.com/document/d/1Ie0hZFwPlQmVHzZLDtyHxviDXtvwY9wNPjnd1Tw8Gy8/edit?tab=t.0#heading=h.fi2ik433erf1)
- **Dependency (Phase 2)**: [RHAISTRAT-1656](https://redhat.atlassian.net/browse/RHAISTRAT-1656) — MaaS BYOIDP (deferred from Phase 1)

### Phase 1 Child Stories

| Story | Title |
|-------|-------|
| [RHOAIENG-62761](https://redhat.atlassian.net/browse/RHOAIENG-62761) | S1 — Controller multi-namespace reconciliation |
| [RHOAIENG-62762](https://redhat.atlassian.net/browse/RHOAIENG-62762) | S2 — MaaSAuthPolicy policy propagation |
| [RHOAIENG-62763](https://redhat.atlassian.net/browse/RHOAIENG-62763) | S3 — DB schema migration (tenant column) |
| [RHOAIENG-62764](https://redhat.atlassian.net/browse/RHOAIENG-62764) | S4 — API key management endpoints |
| [RHOAIENG-62765](https://redhat.atlassian.net/browse/RHOAIENG-62765) | S5 — Internal/gateway contract |
| [RHOAIENG-62766](https://redhat.atlassian.net/browse/RHOAIENG-62766) | S6 — Admission webhook |

## Test Plan Artifacts

- [TestPlan.md](TestPlan.md) — Full test plan (v1.1.0)

## Changelog

| Version | Date | Summary |
|---------|------|---------|
| 1.0.0 | 2026-05-04 | Initial test plan (RHAIRFE-1487 strategy, gateway-per-tenant PoC architecture) |
| 1.1.0 | 2026-05-18 | Major update: Phase 1 scope from RHAISTRAT-1741 + Architecture v2 + S1–S6 stories. Shared-gateway posture, namespace-label isolation, DB migration, API key endpoints, webhook. BYOIDP and per-tenant Gateway deferred to Phase 2. |

## Test Automation

Automated tests will be implemented in the RHOAI collection test repository, covering:
- Tenant namespace provisioning via `maas.opendatahub.io/tenant` label (S1)
- MaaSAuthPolicy → Kuadrant AuthPolicy propagation (S2)
- `api_keys.tenant` column DB migration and rollback (S3)
- `POST /v1/api-keys` and `POST /internal/v1/api-keys/validate` with cross-tenant denial (S4)
- `POST /internal/v1/subscriptions/select` tenant enforcement and `X-MaaS-Tenant` header (S5)
- Admission webhook admit/reject decisions (S6)
- Tenant CR deletion and full resource cleanup

# ModelsAsService Test Plan

**MaaS Team - Model as a Service Testing**

## Document Information

| Field | Value |
|---|---|
| **Feature** | ModelsAsService (MaaS) |
| **Team** | MaaS |
| **Version** | 1.5 |
| **Last Updated** | 2026-04-29 |

---

## 1. Executive Summary

### 1.1 Purpose

This test plan defines the testing approach for the **ModelsAsService** feature in Red Hat
OpenShift AI (RHOAI). MaaS provides a managed model serving platform where users access LLM
inference endpoints through API keys, with access governed by subscriptions, RBAC groups, and
authentication policies enforced at the Kuadrant-based gateway.

### 1.2 Scope

#### In Scope

- **Component Health** - DSC conditions, CRD existence, controller/API deployments and pods,
  subscription stack readiness
- **API Key Management** - CRUD lifecycle, authorization, expiration, ephemeral cleanup, bulk
  operations
- **Subscription Management** - MaaSSubscription CR enforcement, cascade deletion,
  multi-subscription behavior
- **OIDC Authentication** - External OpenID Connect token flows, multi-user key isolation, header
  injection security
- **External Model** - ExternalModel CR lifecycle, credentialed external endpoint routing,
  resource discovery (HTTPRoute/Service), auth enforcement, egress forwarding, OwnerReference
  cleanup

#### Out of Scope

- UI/Dashboard testing (Dashboard team)
- Model deployment and serving runtime (Model Serving team)
- Kuadrant operator internals (Kuadrant team)

### 1.3 Test Objectives

- Validate MaaS component health: DSC conditions, CRDs, deployments, pods, and subscription CR
  readiness
- Validate API key CRUD lifecycle including show-once, revocation, and expiration enforcement
- Verify subscription-based access control and cascade deletion behavior
- Confirm OIDC token authentication with Keycloak and multi-user key isolation
- Validate gateway security: deny-by-default, header injection prevention, auth policy enforcement
- Verify ExternalModel CR reconciliation (HTTPRoute, backend Service) and credentialed egress to
  external inference endpoints

---

## 2. Test Strategy

### 2.1 Test Levels

- **API Integration Testing** - Primary focus, testing REST endpoints against the MaaS API server
- **E2E Functional Testing** - End-to-end flows through the gateway with real authentication
- **Security Testing** - Auth policy enforcement, header injection, token tampering, gateway deny-
  by-default

### 2.2 Test Types

- **Positive Testing** - Valid inputs, expected workflows (e.g., create API key, list models, run
  inference)
- **Negative Testing** - Invalid tokens, unauthorized access, revoked keys, tampered JWTs
- **Boundary Testing** - Expiration limits, rate limit thresholds
- **Security Testing** - Header injection, forged JWTs, IDOR protection, unauthenticated access

### 2.3 Test Priorities

| Priority | Description | Examples |
|---|---|---|
| **P0 (smoke)** | Critical path — must pass for basic functionality | API key creation, model listing, auth enforcement |
| **P1 (tier1)** | Core functional tests | Authorization, bulk ops, subscription enforcement, OIDC model access |
| **P2 (tier2)** | Extended coverage and edge cases | Pagination, double revoke, JWT group claims, expiration boundary |
| **P3 (tier3)** | Security and negative testing | Tampered tokens, empty bearer, forged JWTs |

---

## 3. Test Environment

### 3.1 Test Cluster Configuration

- **OpenShift:** Version 4.14+
- **RHOAI:** Version 3.4+ (with MaaS components)
- **MaaS Components:** maas-api, maas-controller, ModelsAsService CR
- **Models:** TinyLlama (free tier), TinyLlama (premium tier)
- **Python:** 3.11+ (for pytest tests)

### 3.2 Test Users

- **Admin** - OCP cluster admin with full MaaS access
- **Free User** - OCP user, member of `maas-free-group`
- **Premium User** - OCP user, member of `maas-premium-group`
- **OIDC User 1** - Keycloak user (non-admin) from `byoidc-credentials` Secret
- **OIDC User 2** - Second Keycloak user for key isolation tests
- **Service Account** - K8s SA for SA-based access control tests

### 3.3 OIDC-Specific Prerequisites

- Cluster must be a **byoidc** cluster with Keycloak configured
- A dedicated OIDC realm configured in the Keycloak instance with two test users
- The following environment variables set in shift-left vault:
  - `MAAS_OIDC_REALM` (realm name)
  - `MAAS_OIDC_CLIENT_ID` (client ID)
  - `MAAS_OIDC_USER1` / `MAAS_OIDC_PASSWORD1`
  - `MAAS_OIDC_USER2` / `MAAS_OIDC_PASSWORD2`
- `ModelsAsService` CR `externalOIDC` field is patched with issuer URL and client ID
- Authorino TLS must be configured
- `maas-api-auth-policy` AuthPolicy must reach `Enforced` status after OIDC patching
- Disconnected cluster support: TBD

---

## 4. API Endpoints Under Test

### 4.1 API Key Endpoints

| Endpoint | Method | Purpose | Priority |
|---|---|---|---|
| `/v1/api-keys` | POST | Create an API key | P0 |
| `/v1/api-keys/search` | POST | List/search API keys with filters | P1 |
| `/v1/api-keys/{id}` | GET | Get API key details | P1 |
| `/v1/api-keys/{id}` | DELETE | Revoke an API key | P0 |
| `/v1/api-keys/bulk-revoke` | POST | Bulk revoke API keys by username | P1 |
| `/internal/v1/api-keys/cleanup` | POST | Internal ephemeral key cleanup | P1 |

### 4.2 Model & Inference Endpoints

| Endpoint | Method | Purpose | Priority |
|---|---|---|---|
| `/v1/models` | GET | List available models | P0 |
| `/llm/{model}/v1/chat/completions` | POST | Run chat completion inference | P0 |

### 4.3 External Model Endpoints

| Endpoint | Method | Purpose | Priority |
|---|---|---|---|
| `/llm/{external-model}/v1/chat/completions` | POST | Chat completion via credentialed external endpoint | P1 |

### 4.4 Subscription Endpoints

| Endpoint | Method | Purpose | Priority |
|---|---|---|---|
| `/v1/subscriptions` | GET | List user's accessible subscriptions | P1 |
| `/v1/model/{model-id}/subscriptions` | GET | List subscriptions for a specific model | P1 |

---

## 5. Test Cases

All test cases are listed in the index file for organized reference. There are no individual test
case files — all cases are documented in the index.

📋 **Complete Test Case Index:** [test_cases/INDEX.md](test_cases/INDEX.md)

### 5.1 Test Case Organization

| Category | Test Cases | Priority Distribution |
|---|---|---|
| Component Health - API | TC-HEALTH-001 to 004 | P0: 4 |
| Component Health - Controller | TC-HEALTH-005 to 011 | P0: 7 |
| API Key - CRUD | TC-API-001 to 004 | P0: 2, P1: 1, P2: 1 |
| API Key - Authorization | TC-API-005 to 008 | P1: 2, P2: 2 |
| API Key - Auth Policy | TC-API-009 to 010 | P0: 2 |
| API Key - Bulk Operations | TC-API-011 to 013 | P1: 3 |
| API Key - Ephemeral Cleanup | TC-API-014 to 018 | P1: 5 |
| API Key - Expiration | TC-API-019 to 023 | P1: 3, P2: 2 |
| API Key - Gateway Deny Default | TC-API-024 to 025 | P0: 2 |
| API Key - Gateway Rejection | TC-API-026 to 029 | P1: 4 |
| API Key - Creation Negative | TC-API-030 to 033 | P1: 4 |
| Subscription - Cascade Deletion | TC-SUB-001 to 002 | P1: 2 |
| Subscription - List Subscriptions | TC-SUB-003 to 005 | P1: 3 |
| Subscription - List for Model | TC-SUB-006 to 008 | P1: 3 |
| Subscription - Auth Enforcement | TC-SUB-009 to 012 | P0: 3, P1: 1 |
| Subscription - Sub Enforcement | TC-SUB-013 to 015 | P0: 1, P1: 2 |
| Subscription - Multi Auth Policies | TC-SUB-016 to 018 | P1: 3 |
| Subscription - Multi Subscriptions | TC-SUB-019 to 022 | P1: 4 |
| Subscription - Without Auth Policy | TC-SUB-023 | P1: 1 |
| Subscription - Models Endpoint Filtering | TC-SUB-024 to 031 | P1: 6, P2: 2 |
| Subscription - Models Endpoint Access Control | TC-SUB-032 to 034 | P1: 3 |
| Subscription - Models Rate Limit Exemption | TC-SUB-035 | P2: 1 |
| OIDC - Token Flow | TC-OIDC-001 to 005 | P0: 1, P3: 4 |
| OIDC - Model Access | TC-OIDC-006 to 008 | P1: 2, P2: 1 |
| OIDC - Multi-User | TC-OIDC-009 to 013 | P1: 1, P2: 4 |
| OIDC - Header Injection Security | TC-OIDC-014 to 017 | P1: 3, P2: 1 |
| External Model - Auth Enforcement | TC-EXT-001 to 002 | P1: 2 |
| External Model - Discovery | TC-EXT-003 to 006 | P1: 4 |
| External Model - Egress | TC-EXT-007 | P1: 1 |
| External Model - Cleanup | TC-EXT-008 | P2: 1 |

### 5.2 Test Case Naming Convention

Test cases follow the naming pattern: `TC-<CATEGORY>-<NUMBER>`

- `TC-HEALTH-xxx`: Component health testing
- `TC-API-xxx`: API key management testing
- `TC-SUB-xxx`: Subscription management testing
- `TC-OIDC-xxx`: OIDC authentication testing
- `TC-EXT-xxx`: External model testing

---

## 6. Risks and Mitigation

| Risk | Impact | Probability | Mitigation |
|---|---|---|---|
| Keycloak unavailability on byoidc clusters | High | Medium | OIDC tests skip on non-byoidc clusters; separate test runs |
| Rate limit timing sensitivity | Medium | Medium | Use `timeout-sampler` with polling; allow 429-optional for token-rate |
| Subscription controller race conditions | Medium | Low | Fixtures include `wait_for_condition(Ready)` with generous timeouts |
| Gateway propagation delay after revoke | Medium | Medium | Post-revoke tests poll with `TimeoutSampler` (60s) for 401/403 |
| AuthPolicy reconciliation delay | Medium | Medium | Wait for `Enforced=True` condition after patching |
| TLS certificate issues with Authorino | High | Low | `authorino_tls_configured` fixture patches TLS certs on session scope |
| External endpoint unreachable | Medium | Medium | Egress test asserts non-auth-blocking response; httpbin used as stable target |
| HTTPRoute reconciliation delay | Medium | Low | `wait_for_httproute` polls with `TimeoutSampler` (60s) for route creation |

---

## 7. Test Environment Requirements

### 7.1 Infrastructure

- **OpenShift Cluster:** 4.14+ with RHOAI operator
- **MaaS Components:** API server, Gateway, Subscription Controller deployed
- **Authorino:** TLS configured via session-scoped fixture
- **Keycloak:** Required for OIDC tests (byoidc cluster)

### 7.2 Key Fixtures

| Fixture | Scope | Purpose |
|---|---|---|
| `maas_unprivileged_model_namespace` | class | Namespace for model resources |
| `maas_subscription_controller_enabled_latest` | session | Subscription controller enabled |
| `maas_gateway_api` | session | MaaS gateway deployed |
| `maas_api_gateway_reachable` | session | Gateway reachability confirmed |
| `maas_free_group` / `maas_premium_group` | session | OpenShift groups for subscription-based access control |
| `authorino_tls_configured` | session | Authorino TLS patching |
| `oidc_auth_policy_patched` | class | OIDC AuthPolicy patching via ModelsAsService CR |
| `external_model_inference_url` | class | ExternalModel CR, Secret, MaaSModelRef, AuthPolicy, Subscription provisioned |
| `external_model_api_key` | class | API key created for external model subscription; revoked on teardown |

### 7.3 Test Tools

- **Test Framework:** pytest + pytest-testconfig
- **HTTP Client:** requests (via `request_session_http` fixture)
- **Logging:** structlog
- **Polling:** timeout-sampler (for eventual consistency checks)
- **K8s Client:** kubernetes DynamicClient, ocp-resources

---

## 8. Appendix

### 8.1 Test Case Summary

| Category | Total | P0 | P1 | P2 | P3 |
|---|---|---|---|---|---|
| Component Health - API | 4 | 4 | 0 | 0 | 0 |
| Component Health - Controller | 7 | 7 | 0 | 0 | 0 |
| API Key - CRUD | 4 | 2 | 1 | 1 | 0 |
| API Key - Authorization | 4 | 0 | 2 | 2 | 0 |
| API Key - Auth Policy | 2 | 2 | 0 | 0 | 0 |
| API Key - Bulk Operations | 3 | 0 | 3 | 0 | 0 |
| API Key - Ephemeral Cleanup | 5 | 0 | 5 | 0 | 0 |
| API Key - Expiration | 5 | 0 | 3 | 2 | 0 |
| API Key - Gateway Deny Default | 2 | 2 | 0 | 0 | 0 |
| API Key - Gateway Rejection | 4 | 0 | 4 | 0 | 0 |
| API Key - Creation Negative | 4 | 0 | 4 | 0 | 0 |
| Subscription - Cascade Deletion | 2 | 0 | 2 | 0 | 0 |
| Subscription - List Subscriptions | 3 | 0 | 3 | 0 | 0 |
| Subscription - List for Model | 3 | 0 | 3 | 0 | 0 |
| Subscription - Auth Enforcement | 4 | 3 | 1 | 0 | 0 |
| Subscription - Sub Enforcement | 3 | 1 | 2 | 0 | 0 |
| Subscription - Multi Auth Policies | 3 | 0 | 3 | 0 | 0 |
| Subscription - Multi Subscriptions | 4 | 0 | 4 | 0 | 0 |
| Subscription - Without Auth Policy | 1 | 0 | 1 | 0 | 0 |
| Subscription - Models Endpoint Filtering | 8 | 0 | 6 | 2 | 0 |
| Subscription - Models Endpoint Access Control | 3 | 0 | 3 | 0 | 0 |
| Subscription - Models Rate Limit Exemption | 1 | 0 | 0 | 1 | 0 |
| OIDC - Token Flow | 5 | 1 | 0 | 0 | 4 |
| OIDC - Model Access | 3 | 0 | 2 | 1 | 0 |
| OIDC - Multi-User | 5 | 0 | 1 | 4 | 0 |
| OIDC - Header Injection Security | 4 | 0 | 3 | 1 | 0 |
| External Model - Auth Enforcement | 2 | 0 | 2 | 0 | 0 |
| External Model - Discovery | 4 | 0 | 4 | 0 | 0 |
| External Model - Egress | 1 | 0 | 1 | 0 | 0 |
| External Model - Cleanup | 1 | 0 | 0 | 1 | 0 |
| **TOTAL** | **104** | **22** | **63** | **15** | **4** |

### 8.2 API Endpoint Coverage

| Endpoint | Test Cases | Coverage |
|---|---|---|
| POST `/v1/api-keys` | TC-API-001, TC-OIDC-001 to 005, TC-OIDC-014 | ✅ Complete |
| POST `/v1/api-keys/search` | TC-API-002, TC-API-003, TC-API-005 to 008, TC-OIDC-013 | ✅ Complete |
| GET `/v1/api-keys/{id}` | TC-API-001, TC-API-004, TC-API-006 | ✅ Complete |
| DELETE `/v1/api-keys/{id}` | TC-API-004, TC-API-005, TC-OIDC-007, TC-OIDC-008 | ✅ Complete |
| POST `/v1/api-keys/bulk-revoke` | TC-API-011 to 013 | ✅ Complete |
| POST `/internal/v1/api-keys/cleanup` | TC-API-018 | ✅ Complete |
| GET `/v1/models` | TC-API-010, TC-OIDC-006, TC-OIDC-014 to 016 | ✅ Complete |
| POST `/llm/{model}/v1/chat/completions` | TC-SUB-009, TC-SUB-013, TC-OIDC-006, TC-EXT-001, TC-EXT-002, TC-EXT-007 | ✅ Complete |
| GET `/v1/subscriptions` | TC-SUB-003 to 005 | ✅ Complete |
| GET `/v1/model/{model-id}/subscriptions` | TC-SUB-006 to 008 | ✅ Complete |

### 8.3 Document Change Log

| Version | Date | Author | Changes |
|---|---|---|---|
| 1.0 | 2026-04-15 | QA Team | Initial test plan for ModelsAsService — API Key (25 tests), Subscription (23 tests), OIDC (17 tests); 65 test cases total (P0: 11, P1: 39, P2: 11, P3: 4) |
| 1.1 | 2026-04-16 | QA Team | Fixed endpoint path `{model}` → `{model-id}`; Fixed fixture scope `maas_unprivileged_model_namespace` to class; Updated terminology "tier-based" → subscription-based; Removed General/tier-based tests (10 tests); Removed internal endpoints section; Merged OIDC PR #1399; Updated from 75 to 65 test cases |
| 1.2 | 2026-04-17 | QA Team | Added 12 new `/v1/models` endpoint tests (TC-SUB-024 to TC-SUB-035) from PR #1411: filtering (8), access control (3), rate limit exemption (1); Updated from 65 to 77 test cases |
| 1.3 | 2026-04-23 | QA Team | Added 8 ExternalModel tests (TC-EXT-001 to TC-EXT-008) from PR #1446: auth enforcement (2), resource discovery (4), egress forwarding (1), cleanup (1); Added ExternalModel resource class; Updated from 77 to 85 test cases |
| 1.4 | 2026-04-27 | QA Team | Added 11 Component Health tests (TC-HEALTH-001 to TC-HEALTH-011): API health (4), controller health (7); Fixed TC-EXT-008 priority P1 → P2; Fixed TC-SUB-019 file path; Updated from 85 to 96 test cases |
| 1.5 | 2026-04-29 | QA Team | Added 4 API key gateway rejection tests (TC-API-026 to TC-API-029) from PR #1468: malformed key, empty bearer, revoked key, expired key; Added 4 API key creation negative tests (TC-API-030 to TC-API-033) from PR #1487: empty name, invalid subscription, duplicate name, revoked key rejection; Updated from 96 to 104 test cases |

---

*End of Test Plan*

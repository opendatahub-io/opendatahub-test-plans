# MaaS Billing Test Cases Index

This directory contains all test cases for the MaaS Billing feature, organized by category for
easy reference and maintenance.

## Quick Stats

- **Total Test Cases:** 104
- **P0 (Critical):** 22 test cases
- **P1 (High):** 63 test cases
- **P2 (Medium):** 15 test cases
- **P3 (Low):** 4 test cases

## Test Cases by Category

### Component Health - API

| Test Case ID | Title | Priority |
|---|---|---|
| TC-HEALTH-001 | MaaS Management State is MANAGED in DSC | P0 |
| TC-HEALTH-002 | ModelsAsServiceReady Condition is True in DSC | P0 |
| TC-HEALTH-003 | maas-api Deployment Available | P0 |
| TC-HEALTH-004 | maas-api Pods Running and Ready | P0 |

### Component Health - Controller

| Test Case ID | Title | Priority |
|---|---|---|
| TC-HEALTH-005 | ModelsAsServiceReady Condition is True in DSC (Controller) | P0 |
| TC-HEALTH-006 | MaaS Controller CRDs Exist | P0 |
| TC-HEALTH-007 | maas-controller Deployment Available | P0 |
| TC-HEALTH-008 | maas-controller Pods Running | P0 |
| TC-HEALTH-009 | MaaSModelRef Ready for Free Model | P0 |
| TC-HEALTH-010 | MaaSAuthPolicy Ready for Free Model | P0 |
| TC-HEALTH-011 | MaaSSubscription Ready for Free Model | P0 |

### API Key - CRUD

| Test Case ID | Title | Priority |
|---|---|---|
| TC-API-001 | Create API Key and Show-Once Behavior | P0 |
| TC-API-002 | List Active API Keys | P1 |
| TC-API-003 | List API Keys Pagination | P2 |
| TC-API-004 | Revoke API Key | P0 |

### API Key - Authorization

| Test Case ID | Title | Priority |
|---|---|---|
| TC-API-005 | Admin Manages Other User's Keys | P1 |
| TC-API-006 | Non-Admin Cannot Access Other User's Keys (IDOR Protection) | P1 |
| TC-API-007 | Non-Admin Search Returns Only Own Keys | P2 |
| TC-API-008 | Non-Admin Cannot Search by Other Username | P2 |

### API Key - Auth Policy Validation

| Test Case ID | Title | Priority |
|---|---|---|
| TC-API-009 | Auth Policy Callback URL Uses Correct Namespace | P0 |
| TC-API-010 | API Key Can List Models | P0 |

### API Key - Bulk Operations

| Test Case ID | Title | Priority |
|---|---|---|
| TC-API-011 | Bulk Revoke Own Keys | P1 |
| TC-API-012 | Bulk Revoke Other User Forbidden | P1 |
| TC-API-013 | Admin Can Bulk Revoke Any User | P1 |

### API Key - Ephemeral Key Cleanup

| Test Case ID | Title | Priority |
|---|---|---|
| TC-API-014 | CronJob Exists and Configured | P1 |
| TC-API-015 | Cleanup NetworkPolicy Exists | P1 |
| TC-API-016 | Ephemeral Key Visible with Include Filter | P1 |
| TC-API-017 | Ephemeral Key Hidden from Default Search | P1 |
| TC-API-018 | Trigger Cleanup Preserves Active Keys | P1 |

### API Key - Expiration

| Test Case ID | Title | Priority |
|---|---|---|
| TC-API-019 | Create Key Within Expiration Limit | P1 |
| TC-API-020 | Create Key at Expiration Limit | P2 |
| TC-API-021 | Create Key Exceeds Expiration Limit (400) | P1 |
| TC-API-022 | Create Key Without Expiration | P1 |
| TC-API-023 | Create Key with Short Expiration | P2 |

### API Key - Gateway Deny by Default

| Test Case ID | Title | Priority |
|---|---|---|
| TC-API-024 | Gateway Default Auth Exists and Accepted | P0 |
| TC-API-025 | Unconfigured Model Denies Unauthenticated Request | P0 |

### API Key - Gateway Rejection

| Test Case ID | Title | Priority |
|---|---|---|
| TC-API-026 | Malformed API Key Rejected at Inference | P1 |
| TC-API-027 | Empty Bearer Token Rejected at Inference | P1 |
| TC-API-028 | Revoked Key Rejected at Inference | P1 |
| TC-API-029 | Expired Key Rejected at Inference | P1 |

### API Key - Creation Negative

| Test Case ID | Title | Priority |
|---|---|---|
| TC-API-030 | Empty Name Rejected on Create | P1 |
| TC-API-031 | Nonexistent Subscription Rejected on Create | P1 |
| TC-API-032 | Duplicate Name Creation Succeeds | P1 |
| TC-API-033 | Revoked Key Rejected by Model Endpoint | P1 |

### Subscription - Cascade Deletion

| Test Case ID | Title | Priority |
|---|---|---|
| TC-SUB-001 | Delete Subscription Rebuilds TRLP | P1 |
| TC-SUB-002 | Delete Last Subscription Denies Access | P1 |

### Subscription - List Subscriptions

| Test Case ID | Title | Priority |
|---|---|---|
| TC-SUB-003 | Authenticated User Sees Accessible Subscriptions | P1 |
| TC-SUB-004 | Unauthenticated Returns 401 | P1 |
| TC-SUB-005 | Subscription Includes Model Refs with Rate Limits | P1 |

### Subscription - List Subscriptions for Model

| Test Case ID | Title | Priority |
|---|---|---|
| TC-SUB-006 | Returns Only Subscriptions for Requested Model | P1 |
| TC-SUB-007 | Unknown Model Returns Empty List | P1 |
| TC-SUB-008 | Unauthenticated Returns 401 | P1 |

### Subscription - Auth Policy Enforcement

| Test Case ID | Title | Priority |
|---|---|---|
| TC-SUB-009 | Authorized User Gets 200 | P0 |
| TC-SUB-010 | No Auth Header Gets 401 | P0 |
| TC-SUB-011 | Invalid Token Gets 401 | P0 |
| TC-SUB-012 | Wrong Group SA Denied on Premium Model | P1 |

### Subscription - Subscription Enforcement

| Test Case ID | Title | Priority |
|---|---|---|
| TC-SUB-013 | Subscribed User Gets 200 | P0 |
| TC-SUB-014 | Explicit Subscription Header Works | P1 |
| TC-SUB-015 | Invalid Subscription Header Gets 429/403 | P1 |

### Subscription - Multiple Auth Policies per Model

| Test Case ID | Title | Priority |
|---|---|---|
| TC-SUB-016 | Premium Model Denies Free Actor by Default | P1 |
| TC-SUB-017 | Two Auth Policies OR-Logic Allows Access | P1 |
| TC-SUB-018 | Delete Extra Auth Policy Denies Access | P1 |

### Subscription - Multiple Subscriptions

| Test Case ID | Title | Priority |
|---|---|---|
| TC-SUB-019 | Multiple Matching Subscriptions No Header Gets 403 | P1 |
| TC-SUB-020 | User in One of Two Subscriptions Can Access Model | P1 |
| TC-SUB-021 | High Priority Subscription Allows Access When Selected | P1 |
| TC-SUB-022 | SA Cannot Use Subscription It Does Not Belong To | P1 |

### Subscription - Without Auth Policy

| Test Case ID | Title | Priority |
|---|---|---|
| TC-SUB-023 | Subscription Without Auth Policy Gets 403 | P1 |

### Models Endpoint - Filtering (8 tests)

| Test Case ID | Title | Priority |
|---|---|---|
| TC-SUB-024 | API Key Scoped to Subscription | P1 |
| TC-SUB-025 | Unauthenticated Request Returns 401 | P1 |
| TC-SUB-026 | API Key with Deleted Subscription Returns 403 | P1 |
| TC-SUB-027 | Empty Model List Returns Empty Array | P2 |
| TC-SUB-028 | Response Schema Matches OpenAPI | P2 |
| TC-SUB-029 | User Token Returns All Accessible Models | P1 |
| TC-SUB-030 | User Token with Subscription Header Filters | P1 |
| TC-SUB-031 | Invalid Subscription Header Returns 403 | P1 |

### Models Endpoint - Access Control (3 tests)

| Test Case ID | Title | Priority |
|---|---|---|
| TC-SUB-032 | API Key Ignores Injected Subscription Header | P1 |
| TC-SUB-033 | Access Denied to Other User's Subscription (403) | P1 |
| TC-SUB-034 | Single Subscription Auto-Select | P1 |

### Models Endpoint - Rate Limit Exemption (1 test)

| Test Case ID | Title | Priority |
|---|---|---|
| TC-SUB-035 | /v1/models Exempt from Token Rate Limiting | P2 |

### OIDC - Token Flow

| Test Case ID | Title | Priority |
|---|---|---|
| TC-OIDC-001 | OIDC Token Creates API Key | P0 |
| TC-OIDC-002 | Tampered OIDC Token Rejected | P3 |
| TC-OIDC-003 | Empty Bearer Token Rejected | P3 |
| TC-OIDC-004 | No Authorization Header Rejected | P3 |
| TC-OIDC-005 | Random/Forged JWT Rejected | P3 |

### OIDC - Model Access

| Test Case ID | Title | Priority |
|---|---|---|
| TC-OIDC-006 | OIDC Key Lists Models and Runs Inference | P1 |
| TC-OIDC-007 | Revoked OIDC Key Rejected | P1 |
| TC-OIDC-008 | Create Revoke Double Revoke | P2 |

### OIDC - Multi-User

| Test Case ID | Title | Priority |
|---|---|---|
| TC-OIDC-009 | JWT Contains Preferred Username | P1 |
| TC-OIDC-010 | JWT Contains Groups Claim | P2 |
| TC-OIDC-011 | Both Users Have Group Claims | P2 |
| TC-OIDC-012 | Second User Can Mint API Key | P2 |
| TC-OIDC-013 | API Key Isolation Between Users | P2 |

### OIDC - Header Injection Security

| Test Case ID | Title | Priority |
|---|---|---|
| TC-OIDC-014 | Injected X-MaaS-Username Does Not Change Model List | P1 |
| TC-OIDC-015 | Injected X-MaaS-Group Does Not Change Model List | P1 |
| TC-OIDC-016 | Injected X-MaaS-Subscription Does Not Change Model List | P1 |
| TC-OIDC-017 | Injected Username on OIDC Token Ignored | P2 |

### External Model - Auth Enforcement

| Test Case ID | Title | Priority |
|---|---|---|
| TC-EXT-001 | Invalid API Key Returns 401/403 for External Model | P1 |
| TC-EXT-002 | No API Key Returns 401/403 for External Model | P1 |

### External Model - Discovery

| Test Case ID | Title | Priority |
|---|---|---|
| TC-EXT-003 | ExternalModel CR Exists After Creation | P1 |
| TC-EXT-004 | MaaSModelRef CR Exists After Creation | P1 |
| TC-EXT-005 | HTTPRoute Created by Reconciler | P1 |
| TC-EXT-006 | Backend Service Created by Reconciler | P1 |

### External Model - Egress

| Test Case ID | Title | Priority |
|---|---|---|
| TC-EXT-007 | Valid API Key Forwards Request to External Endpoint | P1 |

### External Model - Cleanup

| Test Case ID | Title | Priority |
|---|---|---|
| TC-EXT-008 | Deleting ExternalModel CR Garbage-Collects HTTPRoute | P2 |

## Related Documents

- **Main Test Plan:** [../TestPlan.md](../TestPlan.md)
- **Coverage Assessment:** [../coverage-assessment.md](../coverage-assessment.md)
- **Automation Repo:** [opendatahub-io/opendatahub-tests](<https://github.com/opendatahub-io/>
  opendatahub-tests) (`tests/model_serving/maas_billing/`)

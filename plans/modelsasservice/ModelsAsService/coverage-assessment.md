# MaaS Billing — Test Coverage Assessment

**Date:** 2026-04-29  
**Author:** QA Team

---

## 1. Coverage Summary

The test plan contains **104 test cases** across 5 categories. The table below maps each test case to its automation status and location.

| TC ID | Title | Pri | Where | Status |
|---|---|---|---|---|
| TC-HEALTH-001 | MaaS Management State is MANAGED in DSC | P0 | `component_health/test_maas_api_health_check.py` | ✅ Merged |
| TC-HEALTH-002 | ModelsAsServiceReady Condition is True in DSC | P0 | `component_health/test_maas_api_health_check.py` | ✅ Merged |
| TC-HEALTH-003 | maas-api Deployment Available | P0 | `component_health/test_maas_api_health_check.py` | ✅ Merged |
| TC-HEALTH-004 | maas-api Pods Running and Ready | P0 | `component_health/test_maas_api_health_check.py` | ✅ Merged |
| TC-HEALTH-005 | ModelsAsServiceReady Condition is True in DSC (Controller) | P0 | `component_health/test_maas_controller_health_check.py` | ✅ Merged |
| TC-HEALTH-006 | MaaS Controller CRDs Exist | P0 | `component_health/test_maas_controller_health_check.py` | ✅ Merged |
| TC-HEALTH-007 | maas-controller Deployment Available | P0 | `component_health/test_maas_controller_health_check.py` | ✅ Merged |
| TC-HEALTH-008 | maas-controller Pods Running | P0 | `component_health/test_maas_controller_health_check.py` | ✅ Merged |
| TC-HEALTH-009 | MaaSModelRef Ready for Free Model | P0 | `component_health/test_maas_controller_health_check.py` | ✅ Merged |
| TC-HEALTH-010 | MaaSAuthPolicy Ready for Free Model | P0 | `component_health/test_maas_controller_health_check.py` | ✅ Merged |
| TC-HEALTH-011 | MaaSSubscription Ready for Free Model | P0 | `component_health/test_maas_controller_health_check.py` | ✅ Merged |
| TC-API-001 | Create API Key and Show-Once Behavior | P0 | `maas_api_key/test_api_key_crud.py` | ✅ Merged |
| TC-API-002 | List Active API Keys | P1 | `maas_api_key/test_api_key_crud.py` | ✅ Merged |
| TC-API-003 | List API Keys Pagination | P2 | `maas_api_key/test_api_key_crud.py` | ✅ Merged |
| TC-API-004 | Revoke API Key | P0 | `maas_api_key/test_api_key_crud.py` | ✅ Merged |
| TC-API-005 | Admin Manages Other User's Keys | P1 | `maas_api_key/test_api_key_authorization.py` | ✅ Merged |
| TC-API-006 | Non-Admin Cannot Access Other User's Keys | P1 | `maas_api_key/test_api_key_authorization.py` | ✅ Merged |
| TC-API-007 | Non-Admin Search Returns Only Own Keys | P2 | `maas_api_key/test_api_key_authorization.py` | ✅ Merged |
| TC-API-008 | Non-Admin Cannot Search by Other Username | P2 | `maas_api_key/test_api_key_authorization.py` | ✅ Merged |
| TC-API-009 | Auth Policy Callback URL Uses Correct Namespace | P0 | `maas_api_key/test_api_key_auth_policy_validation.py` | ✅ Merged |
| TC-API-010 | API Key Can List Models | P0 | `maas_api_key/test_api_key_auth_policy_validation.py` | ✅ Merged |
| TC-API-011 | Bulk Revoke Own Keys | P1 | `maas_api_key/test_api_key_bulk_operations.py` | ✅ Merged |
| TC-API-012 | Bulk Revoke Other User Forbidden | P1 | `maas_api_key/test_api_key_bulk_operations.py` | ✅ Merged |
| TC-API-013 | Admin Can Bulk Revoke Any User | P1 | `maas_api_key/test_api_key_bulk_operations.py` | ✅ Merged |
| TC-API-014 | CronJob Exists and Configured | P1 | `maas_api_key/test_api_key_ephemeral_cleanup.py` | ✅ Merged |
| TC-API-015 | Cleanup NetworkPolicy Exists | P1 | `maas_api_key/test_api_key_ephemeral_cleanup.py` | ✅ Merged |
| TC-API-016 | Ephemeral Key Visible with Include Filter | P1 | `maas_api_key/test_api_key_ephemeral_cleanup.py` | ✅ Merged |
| TC-API-017 | Ephemeral Key Hidden from Default Search | P1 | `maas_api_key/test_api_key_ephemeral_cleanup.py` | ✅ Merged |
| TC-API-018 | Trigger Cleanup Preserves Active Keys | P1 | `maas_api_key/test_api_key_ephemeral_cleanup.py` | ✅ Merged |
| TC-API-019 | Create Key Within Expiration Limit | P1 | `maas_api_key/test_api_key_expiration.py` | ✅ Merged |
| TC-API-020 | Create Key at Expiration Limit | P2 | `maas_api_key/test_api_key_expiration.py` | ✅ Merged |
| TC-API-021 | Create Key Exceeds Expiration Limit | P1 | `maas_api_key/test_api_key_expiration.py` | ✅ Merged |
| TC-API-022 | Create Key Without Expiration | P1 | `maas_api_key/test_api_key_expiration.py` | ✅ Merged |
| TC-API-023 | Create Key with Short Expiration | P2 | `maas_api_key/test_api_key_expiration.py` | ✅ Merged |
| TC-API-024 | Gateway Default Auth Exists and Accepted | P0 | `maas_api_key/test_gateway_deny_default.py` | ✅ Merged |
| TC-API-025 | Unconfigured Model Denies Unauthenticated Request | P0 | `maas_api_key/test_gateway_deny_default.py` | ✅ Merged |
| TC-API-026 | Malformed API Key Rejected at Inference | P1 | `maas_api_key/test_api_key_gateway_rejection.py` | ✅ Merged |
| TC-API-027 | Empty Bearer Token Rejected at Inference | P1 | `maas_api_key/test_api_key_gateway_rejection.py` | ✅ Merged |
| TC-API-028 | Revoked Key Rejected at Inference | P1 | `maas_api_key/test_api_key_gateway_rejection.py` | ✅ Merged |
| TC-API-029 | Expired Key Rejected at Inference | P1 | `maas_api_key/test_api_key_gateway_rejection.py` | ✅ Merged |
| TC-API-030 | Empty Name Rejected on Create | P1 | `maas_api_key/test_api_key_creation_negative.py` | ✅ Merged |
| TC-API-031 | Nonexistent Subscription Rejected on Create | P1 | `maas_api_key/test_api_key_creation_negative.py` | ✅ Merged |
| TC-API-032 | Duplicate Name Creation Succeeds | P1 | `maas_api_key/test_api_key_creation_negative.py` | ✅ Merged |
| TC-API-033 | Revoked Key Rejected by Model Endpoint | P1 | `maas_api_key/test_api_key_creation_negative.py` | ✅ Merged |
| TC-SUB-001 | Delete Subscription Rebuilds TRLP | P1 | `maas_subscription/test_cascade_deletion.py` | ✅ Merged |
| TC-SUB-002 | Delete Last Subscription Denies Access | P1 | `maas_subscription/test_cascade_deletion.py` | ✅ Merged |
| TC-SUB-003 | Authenticated User Sees Accessible Subscriptions | P1 | `maas_subscription/test_list_subscriptions.py` | ✅ Merged |
| TC-SUB-004 | Unauthenticated Returns 401 | P1 | `maas_subscription/test_list_subscriptions.py` | ✅ Merged |
| TC-SUB-005 | Subscription Includes Model Refs with Rate Limits | P1 | `maas_subscription/test_list_subscriptions.py` | ✅ Merged |
| TC-SUB-006 | Returns Only Subscriptions for Requested Model | P1 | `maas_subscription/test_list_subscriptions_for_model.py` | ✅ Merged |
| TC-SUB-007 | Unknown Model Returns Empty List | P1 | `maas_subscription/test_list_subscriptions_for_model.py` | ✅ Merged |
| TC-SUB-008 | Unauthenticated Returns 401 | P1 | `maas_subscription/test_list_subscriptions_for_model.py` | ✅ Merged |
| TC-SUB-009 | Authorized User Gets 200 | P0 | `maas_subscription/test_maas_auth_enforcement.py` | ✅ Merged |
| TC-SUB-010 | No Auth Header Gets 401 | P0 | `maas_subscription/test_maas_auth_enforcement.py` | ✅ Merged |
| TC-SUB-011 | Invalid Token Gets 401 | P0 | `maas_subscription/test_maas_auth_enforcement.py` | ✅ Merged |
| TC-SUB-012 | Wrong Group SA Denied on Premium Model | P1 | `maas_subscription/test_maas_auth_enforcement.py` | ✅ Merged |
| TC-SUB-013 | Subscribed User Gets 200 | P0 | `maas_subscription/test_maas_sub_enforcement.py` | ✅ Merged |
| TC-SUB-014 | Explicit Subscription Header Works | P1 | `maas_subscription/test_maas_sub_enforcement.py` | ✅ Merged |
| TC-SUB-015 | Invalid Subscription Header Gets 429/403 | P1 | `maas_subscription/test_maas_sub_enforcement.py` | ✅ Merged |
| TC-SUB-016 | Premium Model Denies Free Actor by Default | P1 | `maas_subscription/test_multiple_auth_policies_per_model.py` | ✅ Merged |
| TC-SUB-017 | Two Auth Policies OR-Logic Allows Access | P1 | `maas_subscription/test_multiple_auth_policies_per_model.py` | ✅ Merged |
| TC-SUB-018 | Delete Extra Auth Policy Denies Access | P1 | `maas_subscription/test_multiple_auth_policies_per_model.py` | ✅ Merged |
| TC-SUB-019 | Multiple Matching Subscriptions No Header Gets 403 | P1 | `maas_subscription/test_multiple_subscriptions_per_model.py` | ✅ Merged |
| TC-SUB-020 | User in One of Two Subscriptions Can Access Model | P1 | `maas_subscription/test_multiple_subscriptions_per_model.py` | ✅ Merged |
| TC-SUB-021 | High Priority Subscription Allows Access When Selected | P1 | `maas_subscription/test_multiple_subscriptions_per_model.py` | ✅ Merged |
| TC-SUB-022 | SA Cannot Use Subscription It Does Not Belong To | P1 | `maas_subscription/test_multiple_subscriptions_per_model.py` | ✅ Merged |
| TC-SUB-023 | Subscription Without Auth Policy Gets 403 | P1 | `maas_subscription/test_subscription_without_auth_policy.py` | ✅ Merged |
| TC-SUB-024 | API Key Scoped to Subscription | P1 | `maas_subscription/test_models_endpoint_filtering.py` | ✅ Merged |
| TC-SUB-025 | Unauthenticated Request Returns 401 | P1 | `maas_subscription/test_models_endpoint_filtering.py` | ✅ Merged |
| TC-SUB-026 | API Key with Deleted Subscription Returns 403 | P1 | `maas_subscription/test_models_endpoint_filtering.py` | ✅ Merged |
| TC-SUB-027 | Empty Model List Returns Empty Array | P2 | `maas_subscription/test_models_endpoint_filtering.py` | ✅ Merged |
| TC-SUB-028 | Response Schema Matches OpenAPI | P2 | `maas_subscription/test_models_endpoint_filtering.py` | ✅ Merged |
| TC-SUB-029 | User Token Returns All Accessible Models | P1 | `maas_subscription/test_models_endpoint_filtering.py` | ✅ Merged |
| TC-SUB-030 | User Token with Subscription Header Filters | P1 | `maas_subscription/test_models_endpoint_filtering.py` | ✅ Merged |
| TC-SUB-031 | Invalid Subscription Header Returns 403 | P1 | `maas_subscription/test_models_endpoint_filtering.py` | ✅ Merged |
| TC-SUB-032 | API Key Ignores Injected Subscription Header | P1 | `maas_subscription/test_models_endpoint_access_control.py` | ✅ Merged |
| TC-SUB-033 | Access Denied to Other User's Subscription (403) | P1 | `maas_subscription/test_models_endpoint_access_control.py` | ✅ Merged |
| TC-SUB-034 | Single Subscription Auto-Select | P1 | `maas_subscription/test_models_endpoint_access_control.py` | ✅ Merged |
| TC-SUB-035 | /v1/models Exempt from Token Rate Limiting | P2 | `maas_subscription/test_models_rate_limit_exemption.py` | ✅ Merged |
| TC-OIDC-001 | OIDC Token Creates API Key | P0 | `oidc_tests/test_oidc_token_flow.py` | ✅ Merged |
| TC-OIDC-002 | Tampered OIDC Token Rejected | P3 | `oidc_tests/test_oidc_token_flow.py` | ✅ Merged |
| TC-OIDC-003 | Empty Bearer Token Rejected | P3 | `oidc_tests/test_oidc_token_flow.py` | ✅ Merged |
| TC-OIDC-004 | No Authorization Header Rejected | P3 | `oidc_tests/test_oidc_token_flow.py` | ✅ Merged |
| TC-OIDC-005 | Random/Forged JWT Rejected | P3 | `oidc_tests/test_oidc_token_flow.py` | ✅ Merged |
| TC-OIDC-006 | OIDC Key Lists Models and Runs Inference | P1 | `oidc_tests/test_oidc_model_access.py` | ✅ Merged |
| TC-OIDC-007 | Revoked OIDC Key Rejected | P1 | `oidc_tests/test_oidc_model_access.py` | ✅ Merged |
| TC-OIDC-008 | Create Revoke Double Revoke | P2 | `oidc_tests/test_oidc_model_access.py` | ✅ Merged |
| TC-OIDC-009 | JWT Contains Preferred Username | P1 | `oidc_tests/test_oidc_multi_user.py` | ✅ Merged |
| TC-OIDC-010 | JWT Contains Groups Claim | P2 | `oidc_tests/test_oidc_multi_user.py` | ✅ Merged |
| TC-OIDC-011 | Both Users Have Group Claims | P2 | `oidc_tests/test_oidc_multi_user.py` | ✅ Merged |
| TC-OIDC-012 | Second User Can Mint API Key | P2 | `oidc_tests/test_oidc_multi_user.py` | ✅ Merged |
| TC-OIDC-013 | API Key Isolation Between Users | P2 | `oidc_tests/test_oidc_multi_user.py` | ✅ Merged |
| TC-OIDC-014 | Injected X-MaaS-Username Does Not Change Model List | P1 | `oidc_tests/test_oidc_header_injection.py` | ✅ Merged |
| TC-OIDC-015 | Injected X-MaaS-Group Does Not Change Model List | P1 | `oidc_tests/test_oidc_header_injection.py` | ✅ Merged |
| TC-OIDC-016 | Injected X-MaaS-Subscription Does Not Change Model List | P1 | `oidc_tests/test_oidc_header_injection.py` | ✅ Merged |
| TC-OIDC-017 | Injected Username on OIDC Token Ignored | P2 | `oidc_tests/test_oidc_header_injection.py` | ✅ Merged |
| TC-EXT-001 | Invalid API Key Returns 401/403 for External Model | P1 | `external_model/test_external_model_auth.py` | ✅ Merged |
| TC-EXT-002 | No API Key Returns 401/403 for External Model | P1 | `external_model/test_external_model_auth.py` | ✅ Merged |
| TC-EXT-003 | ExternalModel CR Exists After Creation | P1 | `external_model/test_external_model_discovery.py` | ✅ Merged |
| TC-EXT-004 | MaaSModelRef CR Exists After Creation | P1 | `external_model/test_external_model_discovery.py` | ✅ Merged |
| TC-EXT-005 | HTTPRoute Created by Reconciler | P1 | `external_model/test_external_model_discovery.py` | ✅ Merged |
| TC-EXT-006 | Backend Service Created by Reconciler | P1 | `external_model/test_external_model_discovery.py` | ✅ Merged |
| TC-EXT-007 | Valid API Key Forwards Request to External Endpoint | P1 | `external_model/test_external_model_egress.py` | ✅ Merged |
| TC-EXT-008 | Deleting ExternalModel CR Garbage-Collects HTTPRoute | P2 | `external_model/test_external_model_cleanup.py` | ✅ Merged |

---

## 2. Summary by Category

| Category | Total | Automated (Merged) | Automated (PR) |
|---|---|---|---|
| Component Health (TC-HEALTH) | 11 | 11 (100%) | 0 |
| API Key (TC-API) | 33 | 33 (100%) | 0 |
| Subscription (TC-SUB) | 35 | 35 (100%) | 0 |
| OIDC (TC-OIDC) | 17 | 17 (100%) | 0 |
| External Model (TC-EXT) | 8 | 8 (100%) | 0 |
| **TOTAL** | **104** | **104 (100%)** | **0** |

---

## 3. Action Items

| # | Priority | Action | Owner | Reference |
|---|---|---|---|---|
No outstanding action items. All 104 test cases are automated and merged.

---

## 4. Verdict

**Component Health area is fully covered** — all 11 test cases are automated and merged.

**API Key area is fully covered** — all 33 test cases are automated and merged.

**Subscription area is fully covered** — all 35 test cases are automated and merged.

**OIDC area is fully covered** — all 17 test cases are automated and merged.

**External Model area is fully covered** — all 8 test cases are automated and merged.

All 104 test cases are automated and merged — **100% coverage**.

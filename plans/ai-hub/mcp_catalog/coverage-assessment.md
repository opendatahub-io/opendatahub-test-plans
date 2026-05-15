# RHOAIENG-44926: MCP Catalog Backend — Test Coverage Evaluation

**Epic:** [RHOAIENG-44926](https://redhat.atlassian.net/browse/RHOAIENG-44926) — MCP Catalog Backend
**Date:** 2026-03-30
**Author:** Federico Mosca

---

## 1. Coverage Summary

The test plan contains **69 test cases** across 6 categories, all automated across upstream Go unit
tests, upstream Python E2E tests, and downstream Python integration tests. The table below maps
each test case to the epic requirement it validates and where it is automated.

| TC ID | Title | Pri | Epic Requirement | Where |
|-------|-------|-----|------------------|-------|
| TC-API-001 | List All MCP Servers | P0 | Discovery | Downstream `test_data_integrity.py`; Upstream Python `test_servers.py` |
| TC-API-002 | List with Pagination | P0 | Pagination | Upstream Python `test_servers.py` |
| TC-API-003 | Filter by Provider | P0 | Filtering | Downstream `test_filtering.py`; Upstream Python `test_filtering.py` |
| TC-API-004 | Filter by Multiple Providers (IN) | P1 | Filtering | Upstream Python `test_filtering.py` |
| TC-API-005 | Filter by Tags (CONTAINS) | P1 | Filtering | Downstream `test_filtering.py` |
| TC-API-006 | Filter by Security Indicators (Boolean) | P1 | Custom properties | Upstream Python `test_filtering.py`; Downstream named query tests |
| TC-API-007 | Filter by Deployment Mode | P1 | Remote servers | Upstream Python `test_remote_servers.py` |
| TC-API-008 | Filter by Transport Type | P1 | Remote servers | Upstream Python `test_remote_servers.py` |
| TC-API-009 | Filter by License (SPDX) | P1 | License metadata | Downstream `test_filtering.py` |
| TC-API-010 | Complex Filter (AND/OR) | P1 | Filtering | Upstream Python `test_filtering.py` |
| TC-API-011 | Named Query Execution | P1 | Filtering | Downstream `test_named_queries.py` |
| TC-API-012 | Keyword Search (q param) | P1 | Search | Downstream `test_keyword_search.py`; Upstream Python `test_q_search.py` |
| TC-API-013 | Combined Search + Filter | P1 | Filtering + Search | Downstream `test_named_queries.py`; `test_keyword_search.py` |
| TC-API-014 | Ordering/Sorting | P1 | Discovery | Downstream `test_ordering.py` |
| TC-API-015 | Filter by Name (Exact) | P0 | Filtering | Upstream Python `test_servers.py` |
| TC-API-016 | Get Server by ID | P0 | Discovery | Upstream Python `test_servers.py`; Upstream Go `TestGetMCPServer` |
| TC-API-017 | Get Non-Existent Server (404) | P1 | Error handling | Upstream Python `test_filtering.py` |
| TC-API-018 | Verify Remote Server Fields | P1 | Remote servers | Upstream Python `test_remote_servers.py` |
| TC-API-019 | List with includeTools=true | P1 | Tools metadata | Downstream `test_data_integrity.py`; Upstream Python `test_servers.py` |
| TC-API-020 | Default excludes tools | P1 | Tools metadata | Downstream `test_data_integrity.py`; Upstream Python `test_servers.py` |
| TC-API-021 | toolLimit parameter | P1 | Tools metadata | Downstream `test_data_integrity.py` (`@xfail` pending RHOAIENG-55737) |
| TC-API-022 | Get single server with tools | P1 | Tools metadata | Upstream Go `TestGetMCPServer` |
| TC-API-023 | Invalid toolLimit | P2 | Tools metadata | Downstream `test_data_integrity.py` (`@xfail` pending RHOAIENG-55737) |
| TC-API-024 | Documentation Fields | P1 | Documentation fields | Upstream Go `mcp_server_test.go` (readme, documentationUrl, repositoryUrl round-trip) |
| TC-API-025 | Transport Derivation for Remote | P1 | Remote servers | Upstream Python `test_remote_servers.py` |
| TC-API-026 | Get Filter Options | P1 | Filtering | Downstream `test_filtering.py` |
| TC-API-027 | List Sources (Default) | P0 | Source configuration | Upstream Python `test_sources.py` |
| TC-API-028 | Sources by assetType=mcp_servers | P0 | Source configuration | Downstream multi-source tests |
| TC-API-029 | Sources by assetType=models | P1 | Source configuration | Upstream Python `test_sources.py` |
| TC-API-030 | List Tools for Server | P1 | Tools metadata | Upstream Go `TestFindMCPServerTools` |
| TC-API-031 | Get Specific Tool | P1 | Tools metadata | Upstream Go `TestGetMCPServerTool` |
| TC-API-032 | Pagination with Filters | P1 | Pagination + Filtering | Downstream `test_filtering.py` |
| TC-API-033 | List Labels (Default) | P0 | Labels | Upstream Python `test_labels.py` |
| TC-API-034 | Labels by assetType=mcp_servers | P0 | Labels | Downstream RHOAIENG-52379 tests |
| TC-API-035 | Labels by assetType=models | P1 | Labels | Upstream Python `test_labels.py` |
| TC-API-036 | Filter by sourceLabel (match) | P0 | Labels + Source config | Downstream `test_source_label.py` |
| TC-API-037 | Filter by sourceLabel (no match) | P1 | Labels + Source config | Downstream `test_source_label.py` |
| TC-API-038 | No sourceLabel returns all | P1 | Labels + Source config | Downstream `test_source_label.py` |
| TC-API-039 | sourceLabel=null (unlabeled) | P1 | Labels + Source config | Downstream `test_source_label.py` |
| TC-DATA-001 | SPDX License Transformations | P1 | License metadata | Upstream Go `providers_test.go::TestYamlMCPServerLicenseTransformation` |
| TC-DATA-002 | Tool Parameters Schema | P1 | Tools metadata | Upstream Go `mcp_server_test.go::TestRoundTrip_OpenapiToolToDbToOpenapi` |
| TC-DATA-003 | Artifact URIs Validated | P1 | URI validation | Upstream Go `validation_test.go`, `yaml_catalog_test.go`, `providers_test.go`; Downstream `test_invalid_yaml.py` |
| TC-DATA-004 | Remote Endpoint URL Validation | P1 | URI validation | Upstream Go `providers_test.go::TestYamlMCPEndpointsValidation` |
| TC-DATA-005 | Custom Properties Metadata Types | P1 | Custom properties | Upstream Go `mcp_server_tool_test.go`; Upstream Python `test_servers.py`; Downstream named queries |
| TC-ERROR-001 | Invalid Filter Query Syntax | P1 | Error handling | Upstream Python `test_filtering.py` |
| TC-ERROR-002 | Invalid Field in Filter | P1 | Error handling | Upstream Go `db_catalog_test.go`, `db_catalog_filterquery_test.go` |
| TC-ERROR-003 | Invalid Page Token | P1 | Error handling | Upstream Go `pagination_test.go::TestNewPaginator_InvalidToken` |
| TC-ERROR-004 | Invalid Page Size | P2 | Error handling | Upstream Go `api_mcp_catalog_service_service_test.go` |
| TC-ERROR-005 | Unknown Named Query | P1 | Error handling | Upstream Python `test_named_queries.py`; Upstream Go `db_mcp_test.go` |
| TC-ERROR-006 | Empty Catalog | P2 | Error handling | Implicit — `ResourceEditor` teardown restores empty ConfigMap; `wait_for_mcp_catalog_api` validates 200 OK |
| TC-ERROR-007 | Special Characters in Search | P2 | Security | Upstream Python `test_q_search.py::test_keyword_search_special_characters` |
| TC-LOAD-001 | Load MCP Servers from YAML | P0 | YAML loading | Downstream `test_data_integrity.py` |
| TC-LOAD-002 | Multiple Sources Loading | P1 | Source configuration | Downstream `test_multi_source.py` |
| TC-LOAD-003 | Glob Include Pattern | P1 | Source configuration | Downstream `test_included_excluded_servers.py` |
| TC-LOAD-004 | Glob Exclude Pattern | P1 | Source configuration | Downstream `test_included_excluded_servers.py` |
| TC-LOAD-005 | Include + Exclude Combined | P1 | Source configuration | Downstream `test_included_excluded_servers.py` |
| TC-LOAD-006 | Field-Level Merging | P1 | Source configuration | Upstream Go unit tests; Downstream `test_multi_source.py` |
| TC-LOAD-007 | Invalid YAML Handling | P1 | Error handling | Downstream `test_invalid_yaml.py` |
| TC-LOAD-008 | Missing Required Fields | P1 | Error handling | Downstream `test_invalid_yaml.py` |
| TC-LOAD-009 | SPDX License Transformation on Load | P1 | License metadata | Upstream Go `providers_test.go::TestYamlMCPServerLicenseTransformation` |
| TC-LOAD-010 | Custom Properties Loading | P1 | Custom properties | Upstream Go `mcp_server_test.go`; Upstream Python `test_servers.py`; Downstream named queries |
| TC-LOAD-011 | Source Cleanup on Disable | P1 | Source configuration | Downstream `test_multi_source.py` |
| TC-LOAD-012 | Source Cleanup on Removal | P1 | Source configuration | Downstream `test_multi_source.py` |
| TC-LOAD-013 | Timestamp Preservation | P1 | YAML loading | Downstream `test_data_integrity.py` |
| TC-LOAD-014 | Named Query Merging | P1 | Source configuration | Upstream Go `db_mcp_test.go`; Downstream `test_named_queries.py` |
| TC-REG-001 | Model Catalog Non-Regression | P0 | Regression | Separate model catalog test suite in CI |
| TC-REG-002 | Catalog Service Startup | P0 | Regression | Implicit — all tests fail if service doesn't start |
| TC-SEC-001 | Unauthenticated Access Denied | P0 | Security | Downstream `test_security.py::test_unauthenticated_access_denied` |
| TC-SEC-002 | SQL Injection Prevention | P0 | Security | Upstream Go `TestFilterQuerySQLInjectionPrevention`; Upstream Python `test_keyword_search_special_characters` |

**Additional coverage not in the test plan** (dev story bugs with upstream Go tests):

| Area | Dev Story | Where |
|------|-----------|-------|
| K8s prerequisites metadata | RHOAIENG-54413 | Upstream Go `mcp_server_prerequisites_test.go` |
| Tool accessType & inputParameters | RHOAIENG-54119 | Upstream Go `loader_test.go::TestMCPLoaderToolAccessTypeAndParameters` |
| securityIndicators from YAML | RHOAIENG-54715 | Upstream Go `providers_test.go::TestYamlMCPServerSecurityIndicatorsToStandardProperties` |

| Category | Total | Automated |
|----------|-------|-----------|
| API (TC-API) | 39 | 39 (100%) |
| DATA (TC-DATA) | 5 | 5 (100%) |
| ERROR (TC-ERROR) | 7 | 7 (100%) |
| LOAD (TC-LOAD) | 14 | 14 (100%) |
| REG (TC-REG) | 2 | 2 (100%) |
| SEC (TC-SEC) | 2 | 2 (100%) |
| **TOTAL** | **69** | **69 (100%)** |

---

## 2. Action Items

| # | Priority | Action | Owner | Jira |
|---|----------|--------|-------|------|
| 1 | P0 | Remove `@xfail` from `test_mcp_server_tool_limit` and `test_tool_limit_exceeding_maximum` once RHOAIENG-55737 is fixed | Federico | RHOAIENG-55737 |
| 2 | P1 | Unblock RHOAIENG-52181 — run upstream Go tests on real cluster (still "New") | Debarati | RHOAIENG-52181 |

---

## 3. Verdict

**The epic is fully covered.** All 69 test plan cases are automated (100%) across upstream Go,
upstream Python, and downstream Python. Every functional area the backend delivers (discovery,
filtering, pagination, tools, remote servers, sources, labels, licensing, security, error handling,
K8s prerequisites, securityIndicators, URI validation) is validated. All 20 dev stories/bugs —
including the 7 without dedicated testing tickets — have automated coverage through existing
tests.

### Are additional tests beyond the test plan needed?

**No.** The following areas were evaluated as potential gaps and found to be either already covered
or not applicable:

| Area | Assessment | Why |
|------|------------|-----|
| API endpoints | No gap | All 7 endpoints have positive, negative, and edge case coverage |
| Bug fix regressions | No gap | RHOAIENG-54119, 54555, 54579, 54715 all have upstream Go tests |
| New data model fields | No gap | K8s prerequisites, securityIndicators, documentation fields, tool parameters all have round-trip tests |
| RBAC / multi-user | Not applicable | API is read-only — all authenticated users have same access; TC-SEC-001 covers unauthenticated denial |
| Operator ConfigMap setup | Out of scope | Covered by its own testing ticket RHOAIENG-54874 |
| Performance / scale | Out of scope | Explicitly deferred for v1 per test plan |

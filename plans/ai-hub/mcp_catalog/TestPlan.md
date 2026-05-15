# MCP Catalog Backend Test Plan

**AI Hub Team - Backend API Testing**

## Document Information

- **Feature**: MCP Catalog – Enterprise Control of MCP Assets
- **Epic**: [RHOAIENG-44926](https://issues.redhat.com/browse/RHOAIENG-44926)
- **Feature Story**: [RHAISTRAT-1084](https://issues.redhat.com/browse/RHAISTRAT-1084)
- **ADR**: [MCP Catalog Backend ADR](<https://docs.google.com/document/d/17PBwS5DzDI79eGXUkHowmlQlVSK>
  OwXbBl__DtNRAwfU/edit?usp=sharing) (January 7, 2026)
- **Team**: AI Hub
- **Version**: 1.9.1
- **Last Updated**: 2026-03-27

---

## 1. Executive Summary

### 1.1 Purpose

This test plan defines the testing approach for the **MCP Catalog Backend** delivered by the AI
Hub team. The backend provides REST API endpoints for discovering, browsing, and retrieving MCP
(Model Context Protocol) server metadata from configured catalog sources.

### 1.2 Scope

#### In Scope (AI Hub Backend Responsibilities)

- **MCP Catalog API Endpoints** - REST APIs for listing, filtering, and retrieving MCP servers
- **YAML Catalog Loading** - Reading MCP server definitions from YAML configuration files
- **Database Storage** - Persisting MCP server metadata in ml_metadata database
- **Catalog Sources API** - Managing and querying catalog sources (models + MCP servers)
- **Filtering & Search** - Server-side filtering, named queries, keyword search
- **Source Configuration** - Multi-source support with field-level merging and glob filtering
- **Data Models** - MCP server entities, tools, artifacts, endpoints, security indicators
- **License Metadata** - SPDX identifier support and display name transformation
- **Custom Properties** - Support for tags and security indicators via custom properties

#### Out of Scope (Other Teams)

- **UI Dashboard** - MCP Catalog UI, deployment wizard, deployed servers view (Dashboard team)
- **MCP Gateway/MCPServer CRDs** - Runtime MCP server infrastructure (Agentic MCP team)
- **Deployment Workflow** - Kubernetes resource creation, helm charts (MCP team)
- **genAI Studio Integration** - Playground UI, ConfigMap management (UI team)
- **Tool Invocation** - MCP tool execution and testing (out of scope per feature doc)

### 1.3 Test Objectives

1. Validate all MCP Catalog REST API endpoints return correct data
2. Verify YAML catalog loading populates database correctly
3. Confirm filtering, search, and pagination work as specified
4. Validate includeTools and toolLimit parameters for controlling tools in responses
5. Validate source merging and filtering configuration
6. Ensure SPDX license transformation and custom properties handling
7. Test API error handling and edge cases

---

## 2. Test Strategy

### 2.1 Test Levels

- **API Integration Testing** - Primary focus, testing REST endpoints against database
- **Data Validation Testing** - YAML loading, data transformation, database persistence
- **Functional Testing** - Business logic, filtering, search, pagination
- **Security Testing** - Authentication, authorization (basic validation)

### 2.2 Test Types

- **Positive Testing** - Valid inputs, expected workflows
- **Negative Testing** - Invalid inputs, error conditions, edge cases
- **Boundary Testing** - Pagination limits, filter combinations, large datasets
- **Regression Testing** - Ensure existing functionality remains intact

### 2.3 Test Priorities

- **P0 (Critical)** - Core API endpoints, catalog loading, basic filtering
- **P1 (High)** - Advanced filtering, search, pagination, source merging
- **P2 (Medium)** - Edge cases, optional features

### 2.4 Performance Baseline Measurement

While there are no formal performance requirements, response times should be measured and recorded
to establish a performance baseline:

**Measurement Points:**

- List MCP servers (small dataset: <100 servers)
- List MCP servers (large dataset: 500+ servers)
- Get single MCP server by ID
- Complex filter queries
- Search queries

**Purpose:**

- Establish baseline performance characteristics
- Detect performance regressions in future releases
- Provide data for future performance requirement discussions
- Identify potential optimization opportunities

**Recording:**
Response times should be documented in test execution reports but are informational only (no
pass/fail criteria).

---

## 3. Test Environment

### 3.1 Test Cluster Configuration

- **OpenShift**: Version 4.14+
- **RHOAI**: Version 3.4+ (with AI Hub components)
- **Database**: ml_metadata (PostgreSQL-based)
- **Python**: 3.11+ (for pytest tests)

### 3.2 Test Data Requirements

#### Catalog Sources Configuration

```yaml
# Test catalog sources (dev-mcp-catalog-sources.yaml)
catalogs:
  - id: organization_mcp_servers
    name: "Organization MCP Servers"
    type: yaml
    enabled: true
    labels: ["Validated", "Enterprise"]
    properties:
      yamlCatalogPath: organization-mcp-servers.yaml
    includedServers: ["github-*", "slack-*", "dynatrace-*"]
    excludedServers: ["*-alpha", "*-deprecated"]
```

### 3.3 Test Users

- **API Client** - Service account with catalog read permissions
- **Admin Client** - Admin user for full API access

---

## 4. API Endpoints Under Test

### 4.1 MCP Server Endpoints

| Endpoint | Method | Purpose | Priority |
|----------|--------|---------|----------|
| `/api/model_catalog/v1alpha1/mcp_servers` | GET | List MCP servers with filtering | P0 |
| `/api/model_catalog/v1alpha1/mcp_servers/{id}` | GET | Get specific MCP server by ID | P0 |
| `/api/model_catalog/v1alpha1/mcp_servers/{id}/tools` | GET | List tools for specific MCP server | P1 |
| `/api/model_catalog/v1alpha1/mcp_servers/{id}/tools/{name}` | GET | Get specific tool from MCP server | P1 |
| `/api/model_catalog/v1alpha1/mcp_servers/filter_options` | GET | Get available filter fields | P1 |

### 4.2 Catalog Sources Endpoint

| Endpoint | Method | Purpose | Priority |
|----------|--------|---------|----------|
| `/api/model_catalog/v1alpha1/sources` | GET | List catalog sources (models + MCP) | P0 |

### 4.3 Catalog Labels Endpoint

| Endpoint | Method | Purpose | Priority |
|----------|--------|---------|----------|
| `/api/model_catalog/v1alpha1/labels` | GET | List catalog labels filtered by assetType | P0 |

---

## 5. Test Cases

All test cases have been extracted into individual markdown files for better organization and
maintainability.

**📁 Test Cases Directory**: [test_cases/](test_cases/)

**📋 Complete Test Case Index**: [test_cases/INDEX.md](test_cases/INDEX.md)

### 5.1 Test Case Organization

Test cases are organized by category and stored as individual markdown files in the `test_cases/`
directory:

| Category | Test Cases | Priority Distribution |
|----------|------------|----------------------|
| **API - Listing & Filtering** | TC-API-001 to 015, TC-API-032, TC-API-036 to 039 | P0: 5, P1: 14, P2: 1 |
| **API - Server Details** | TC-API-016 to 018 | P0: 1, P1: 2 |
| **API - Tools Parameters** | TC-API-019 to 025 | P1: 6, P2: 1 |
| **API - Tools Endpoints** | TC-API-030 to 031 | P1: 2 |
| **API - Filter Options** | TC-API-026 | P1: 1 |
| **API - Catalog Sources** | TC-API-027 to 029 | P0: 2, P1: 1 |
| **API - Catalog Labels** | TC-API-033 to 035 | P0: 2, P1: 1 |
| **YAML Loading** | TC-LOAD-001 to 014 | P0: 1, P1: 12, P2: 1 |
| **Error Handling** | TC-ERROR-001 to 007 | P1: 5, P2: 2 |
| **Data Validation** | TC-DATA-001 to 005 | P1: 5 |
| **Security Testing** | TC-SEC-001 to 002 | P0: 2 |
| **Regression Testing** | TC-REG-001 to 002 | P0: 2 |

### 5.2 Test Case Naming Convention

Test cases follow the naming pattern: `TC-<CATEGORY>-<NUMBER>`

- **TC-API-xxx**: API endpoint testing
- **TC-LOAD-xxx**: YAML loading and catalog configuration
- **TC-ERROR-xxx**: Error handling and edge cases
- **TC-DATA-xxx**: Data validation and transformation
- **TC-SEC-xxx**: Security and authentication testing
- **TC-REG-xxx**: Regression and compatibility testing

---

## 6. Risks and Mitigation

| Risk | Impact | Probability | Mitigation |
|------|--------|-------------|------------|
| **Dependency on MCP Gateway CRDs** | High | Medium | Test backend independently; mock Gateway integration if needed |
| **Database schema changes** | Medium | Low | Coordinate with dev team on schema; update tests accordingly |
| **Test data unavailable** | Medium | Low | Create comprehensive YAML test fixtures early |
| **API contract changes late** | High | Medium | Lock OpenAPI spec early; automated contract testing |
| **Performance issues at scale** | Medium | Medium | Load test with large catalogs (500+ servers) early |

---

## 7. Test Environment Requirements

### 10.1 Infrastructure

- **OpenShift Cluster**: 4.14+
- **RHOAI**: 3.4 with AI Hub components deployed
- **Database**: PostgreSQL (via ml_metadata)
- **Catalog Service**: AI Hub backend with MCP catalog enabled

### 10.2 Configuration

- **Catalog Sources**: 2-3 sources configured (org, community)
- **YAML Files**: Located at `/etc/catalog/` or configurable path
- **Environment Variables**:
  - `CATALOG_SOURCES_PATH=/etc/catalog/dev-mcp-catalog-sources.yaml`
  - Database connection settings

### 10.3 Test Tools

- **API Testing**: curl, Postman, or pytest + requests
- **Database Query**: psql or database admin tool
- **Log Viewing**: oc logs, kubectl logs
- **Performance**: time, Apache Bench, or custom tooling

---

## 8. Appendix

### 8.1 Test Case Summary

| Category | Total | P0 | P1 | P2 |
|----------|-------|----|----|-----|
| API - Listing & Filtering | 20 | 5 | 14 | 1 |
| API - Server Details | 3 | 1 | 2 | 0 |
| API - Tools Parameters | 7 | 0 | 6 | 1 |
| API - Tools Endpoints | 2 | 0 | 2 | 0 |
| API - Filter Options | 1 | 0 | 1 | 0 |
| API - Catalog Sources | 3 | 2 | 1 | 0 |
| API - Catalog Labels | 3 | 2 | 1 | 0 |
| YAML Loading | 14 | 1 | 12 | 1 |
| Error Handling | 7 | 0 | 5 | 2 |
| Data Validation | 5 | 0 | 5 | 0 |
| Security | 2 | 2 | 0 | 0 |
| Regression | 2 | 2 | 0 | 0 |
| **TOTAL** | **69** | **15** | **49** | **5** |

### 8.2 API Endpoint Coverage

| Endpoint | Test Cases | Coverage |
|----------|------------|----------|
| `GET /mcp_servers` | TC-API-001 to 015, 019-021, 032, 036-039 | ✅ Complete |
| `GET /mcp_servers/{id}` | TC-API-016 to 018, 022 | ✅ Complete |
| `GET /mcp_servers/{id}/tools` | TC-API-030 | ✅ Complete |
| `GET /mcp_servers/{id}/tools/{name}` | TC-API-031 | ✅ Complete |
| `GET /mcp_servers/filter_options` | TC-API-026 | ✅ Complete |
| `GET /sources` | TC-API-027 to 029 | ✅ Complete |
| `GET /labels` | TC-API-033 to 035 | ✅ Complete |

**Note**: Tools can be accessed via `includeTools` and `toolLimit` parameters on server endpoints,
or via dedicated `/tools` endpoints for detailed tool listing.

### 8.3 Document Change Log

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0 | 2026-02-09 | QA Team | Initial test plan for AI Hub backend |
| 1.1 | 2026-02-09 | QA Team | Aligned with ADR: Removed separate tools endpoints, added includeTools/toolLimit parameters, added 7 new test cases (TC-API-034 to 036, TC-LOAD-011 to 014) |
| 1.2 | 2026-02-09 | QA Team | Restructured: Added ADR reference; Extracted all 64 test cases to individual markdown files in test_cases/ directory for better organization and maintainability |
| 1.3 | 2026-02-09 | QA Team | Simplified: Removed sections; Focused on test strategy and cases only |
| 1.4 | 2026-02-09 | QA Team | Removed performance test cases (TC-PERF-001 to 004) as no performance requirements exist; Removed TC-SEC-002 (write operations) as backend has no write endpoints; Enhanced TC-LOAD-001 with database validation; Added section 2.4 Performance Baseline Measurement; Updated from 64 to 59 test cases (P0: 12, P1: 42, P2: 5) |
| 1.4.1 | 2026-02-11 | fmosca | Updated INDEX.md format - Test Case ID column now contains clickable links to test case files, removed redundant File column |
| 1.5 | 2026-02-11 | fmosca | Added 2 new test cases for tools endpoints from ADR: TC-API-030 (List Tools for MCP Server), TC-API-031 (Get Specific Tool); Updated from 59 to 61 test cases (P0: 12, P1: 44, P2: 5) |
| 1.6 | 2026-03-11 | fmosca | Added TC-API-032 (Pagination Combined with Filter Query) for RHOAIENG-51584; Updated from 61 to 62 test cases (P0: 12, P1: 45, P2: 5) |
| 1.6.1 | 2026-03-13 | fmosca | Marked TC-LOAD-006 and TC-LOAD-014 as covered by upstream Go unit tests (RHOAIENG-46613); No downstream automation needed — merge logic tested in `mcp_source_merge_test.go`, no API to verify source-level merge results |
| 1.6.2 | 2026-03-13 | fmosca | RHOAIENG-52376: Fixed TC-API-008 filter syntax (`CONTAINS` → `=` operator); Fixed TC-API-018/TC-API-025 field names (`httpUrl` → `http`, `sseUrl` → `sse`); Marked TC-DATA-004 as covered by upstream Go unit tests (`TestYamlMCPEndpointsValidation`) |
| 1.7 | 2026-03-17 | fmosca | RHOAIENG-52247: Added 3 new test cases for `/labels` endpoint `assetType` query parameter: TC-API-033 (List Labels Default Behavior, P0), TC-API-034 (Filter Labels by assetType=mcp_servers, P0), TC-API-035 (Filter Labels by assetType=models, P1); Added section 4.3 (Catalog Labels Endpoint); Updated from 62 to 65 test cases (P0: 12→14, P1: 45→46) |
| 1.8 | 2026-03-20 | fmosca | RHOAIENG-52381: Updated coverage for TC-DATA-003 (out of scope — product gap, bug RHOAIENG-54555), TC-DATA-005 (covered upstream by Go converter tests + Python `test_custom_properties_loaded`), TC-API-024 (covered upstream — mandatory fields validated, optional fields are `omitempty`) |
| 1.9 | 2026-03-24 | fmosca | RHOAIENG-54108: Added 4 new test cases for MCP `sourceLabel` query parameter: TC-API-036 (Filter by matching source, P0), TC-API-037 (No match returns empty, P1), TC-API-038 (No sourceLabel returns all, P1), TC-API-039 (sourceLabel=null for unlabeled sources, P1); Updated from 65 to 69 test cases (P0: 14→15, P1: 46→49) |
| 1.9.1 | 2026-03-27 | fmosca | RHOAIENG-52453: Marked TC-API-022, TC-API-030, TC-API-031 as covered by upstream Go unit tests (`TestGetMCPServer`, `TestFindMCPServerTools`, `TestGetMCPServerTool`); No downstream automation needed for these three |
| 1.10 | 2026-03-30 | fmosca | Added [coverage-assessment.md](coverage-assessment.md): full coverage assessment confirming all 69 test cases are automated (100%) across upstream Go, upstream Python, and downstream Python; all 20 epic dev stories/bugs validated; no additional tests needed |

---

**End of Test Plan**

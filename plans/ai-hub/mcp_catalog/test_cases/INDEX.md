# MCP Catalog Test Cases Index

This directory contains all test cases for the MCP Catalog Backend feature, organized as individual
markdown files for easy reference and maintenance.

## Quick Stats

- **Total Test Cases**: 69
- **P0 (Critical)**: 15 test cases
- **P1 (High)**: 49 test cases
- **P2 (Medium)**: 5 test cases

## Test Cases by Category

### API Testing

| Test Case ID | Title | Priority |
|--------------|-------|----------|
| [TC-API-001](TC-API-001.md) | List All MCP Servers | P0 |
| [TC-API-002](TC-API-002.md) | List MCP Servers with Pagination | P0 |
| [TC-API-003](TC-API-003.md) | Filter MCP Servers by Provider | P0 |
| [TC-API-004](TC-API-004.md) | Filter MCP Servers by Multiple Providers (IN operator) | P1 |
| [TC-API-005](TC-API-005.md) | Filter MCP Servers by Tags (CONTAINS operator) | P1 |
| [TC-API-006](TC-API-006.md) | Filter MCP Servers by Security Indicators (Boolean) | P1 |
| [TC-API-007](TC-API-007.md) | Filter MCP Servers by Deployment Mode | P1 |
| [TC-API-008](TC-API-008.md) | Filter MCP Servers by Transport Type | P1 |
| [TC-API-009](TC-API-009.md) | Filter MCP Servers by License (SPDX) | P1 |
| [TC-API-010](TC-API-010.md) | Complex Filter Query (Multiple Conditions with AND/OR) | P1 |
| [TC-API-011](TC-API-011.md) | Named Query Execution | P1 |
| [TC-API-012](TC-API-012.md) | Keyword Search (q parameter) | P1 |
| [TC-API-013](TC-API-013.md) | Combined Search and Filter | P1 |
| [TC-API-014](TC-API-014.md) | Ordering and Sorting | P1 |
| [TC-API-015](TC-API-015.md) | Filter by Name (Exact Match) | P0 |
| [TC-API-016](TC-API-016.md) | Get MCP Server by ID | P0 |
| [TC-API-017](TC-API-017.md) | Get Non-Existent MCP Server (404) | P1 |
| [TC-API-018](TC-API-018.md) | Verify Remote Server Fields | P1 |
| [TC-API-019](TC-API-019.md) | List MCP Servers with Tools Included | P1 |
| [TC-API-020](TC-API-020.md) | List MCP Servers with Tools Excluded (Default) | P1 |
| [TC-API-021](TC-API-021.md) | Tool Limit Parameter | P1 |
| [TC-API-022](TC-API-022.md) | Get Single Server with Tools (upstream unit tests) | P1 |
| [TC-API-023](TC-API-023.md) | Invalid toolLimit Parameter Validation | P2 |
| [TC-API-024](TC-API-024.md) | Documentation Fields Validation | P1 |
| [TC-API-025](TC-API-025.md) | Transport Derivation for Remote Servers | P1 |
| [TC-API-026](TC-API-026.md) | Get Filter Options | P1 |
| [TC-API-027](TC-API-027.md) | List All Catalog Sources | P0 |
| [TC-API-028](TC-API-028.md) | Filter Catalog Sources by Asset Type (MCP) | P0 |
| [TC-API-029](TC-API-029.md) | Filter Catalog Sources by Asset Type (Models) | P1 |
| [TC-API-030](TC-API-030.md) | List Tools for MCP Server (upstream unit tests) | P1 |
| [TC-API-031](TC-API-031.md) | Get Specific Tool from MCP Server (upstream unit tests) | P1 |
| [TC-API-032](TC-API-032.md) | Pagination Combined with Filter Query | P1 |
| [TC-API-033](TC-API-033.md) | List Catalog Labels (Default Behavior) | P0 |
| [TC-API-034](TC-API-034.md) | Filter Catalog Labels by Asset Type (MCP) | P0 |
| [TC-API-035](TC-API-035.md) | Filter Catalog Labels by Asset Type (Models) | P1 |
| [TC-API-036](TC-API-036.md) | Filter MCP Servers by sourceLabel (Matching Source) | P0 |
| [TC-API-037](TC-API-037.md) | Filter MCP Servers by sourceLabel (No Match Returns Empty) | P1 |
| [TC-API-038](TC-API-038.md) | List MCP Servers Without sourceLabel Returns All | P1 |
| [TC-API-039](TC-API-039.md) | Filter MCP Servers by sourceLabel=null (Unlabeled Sources) | P1 |

### Data Validation

| Test Case ID | Title | Priority |
|--------------|-------|----------|
| [TC-DATA-001](TC-DATA-001.md) | Verify All SPDX License Transformations | P1 |
| [TC-DATA-002](TC-DATA-002.md) | Verify Tool Parameters Schema | P1 |
| [TC-DATA-003](TC-DATA-003.md) | Verify Artifact URIs Validated | P1 |
| [TC-DATA-004](TC-DATA-004.md) | Verify Remote Endpoint URL Validation (upstream unit tests) | P1 |
| [TC-DATA-005](TC-DATA-005.md) | Verify Custom Properties Metadata Types | P1 |

### Error Handling

| Test Case ID | Title | Priority |
|--------------|-------|----------|
| [TC-ERROR-001](TC-ERROR-001.md) | Invalid Filter Query Syntax | P1 |
| [TC-ERROR-002](TC-ERROR-002.md) | Invalid Field Name in Filter | P1 |
| [TC-ERROR-003](TC-ERROR-003.md) | Invalid Page Token | P1 |
| [TC-ERROR-004](TC-ERROR-004.md) | Invalid Page Size (Out of Range) | P2 |
| [TC-ERROR-005](TC-ERROR-005.md) | Unknown Named Query | P1 |
| [TC-ERROR-006](TC-ERROR-006.md) | Empty Catalog (No Servers) | P2 |
| [TC-ERROR-007](TC-ERROR-007.md) | Special Characters in Search Query | P2 |

### YAML Loading

| Test Case ID | Title | Priority |
|--------------|-------|----------|
| [TC-LOAD-001](TC-LOAD-001.md) | Load MCP Servers from YAML | P0 |
| [TC-LOAD-002](TC-LOAD-002.md) | YAML Loading with Multiple Sources | P1 |
| [TC-LOAD-003](TC-LOAD-003.md) | YAML Loading with Glob Filtering (Include) | P1 |
| [TC-LOAD-004](TC-LOAD-004.md) | YAML Loading with Glob Filtering (Exclude) | P1 |
| [TC-LOAD-005](TC-LOAD-005.md) | YAML Loading with Include and Exclude Combined | P1 |
| [TC-LOAD-006](TC-LOAD-006.md) | Field-Level Merging from Multiple Config Files (upstream unit tests) | P1 |
| [TC-LOAD-007](TC-LOAD-007.md) | Invalid YAML Handling | P1 |
| [TC-LOAD-008](TC-LOAD-008.md) | Missing Required Fields in YAML | P1 |
| [TC-LOAD-009](TC-LOAD-009.md) | SPDX License Transformation on Load | P1 |
| [TC-LOAD-010](TC-LOAD-010.md) | Custom Properties Loading (Tags and Booleans) | P1 |
| [TC-LOAD-011](TC-LOAD-011.md) | Source Cleanup on Disable | P1 |
| [TC-LOAD-012](TC-LOAD-012.md) | Source Cleanup on Removal | P1 |
| [TC-LOAD-013](TC-LOAD-013.md) | Timestamp Preservation from YAML | P1 |
| [TC-LOAD-014](TC-LOAD-014.md) | Named Query Merging from Multiple Config Files (upstream unit tests) | P1 |

### Regression Testing

| Test Case ID | Title | Priority |
|--------------|-------|----------|
| [TC-REG-001](TC-REG-001.md) | Model Catalog Still Works (Non-Regression) | P0 |
| [TC-REG-002](TC-REG-002.md) | Catalog Service Startup | P0 |

### Security Testing

| Test Case ID | Title | Priority |
|--------------|-------|----------|
| [TC-SEC-001](TC-SEC-001.md) | Unauthenticated Access Denied | P0 |
| [TC-SEC-002](TC-SEC-002.md) | SQL Injection Prevention | P0 |

## Related Documents

- **Main Test Plan**: [../TestPlan.md](../TestPlan.md)
- **Feature Epic**: [RHOAIENG-44926](https://issues.redhat.com/browse/RHOAIENG-44926)
- **ADR**: [MCP Catalog Backend ADR](<https://docs.google.com/document/d/17PBwS5DzDI79eGXUkHowmlQlVSK>
  OwXbBl__DtNRAwfU/edit?usp=sharing)

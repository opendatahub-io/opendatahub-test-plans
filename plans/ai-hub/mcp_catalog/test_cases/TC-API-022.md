# TC-API-022: Get Single Server with Tools

**Priority**: P1
**Objective**: Verify tools excluded by default for single server GET
**Automation Status**: Covered by upstream Go unit tests (`TestGetMCPServer` in
`api_mcp_catalog_service_service_test.go`, BFF `TestGetMcpServer_WithIncludeTools` in
`mcp_server_catalog_test.go`)

**Test Steps**:

1. Send GET request to `/api/model_catalog/v1alpha1/mcp_servers/{id}`
2. Verify response does NOT include tools array by default
3. Verify other metadata is complete

**Expected Results**:

- Status: 200 OK
- Tools array not included
- All other metadata present

**Test Data**:

```bash
GET /api/model_catalog/v1alpha1/mcp_servers/1
```

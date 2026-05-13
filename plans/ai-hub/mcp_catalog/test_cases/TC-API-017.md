# TC-API-017: Get Non-Existent MCP Server (404)

**Priority**: P1
**Objective**: Verify error handling for non-existent server ID

**Test Steps**:

1. Send GET request to `/api/model_catalog/v1alpha1/mcp_servers/99999`
2. Verify response status is 404 Not Found
3. Verify error message is descriptive

**Expected Results**:

- Status: 404 Not Found
- Error message: "MCP server with ID 99999 not found"

**Test Data**:

```bash
GET /api/model_catalog/v1alpha1/mcp_servers/99999
```

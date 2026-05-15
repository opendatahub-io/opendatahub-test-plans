# TC-API-031: Get Specific Tool from MCP Server

**Priority**: P1
**Objective**: Verify retrieval of a specific tool by name from an MCP server
**Automation Status**: Covered by upstream Go unit tests (`TestGetMCPServerTool` in `api_mcp_catalog_service_service_test.go`)

**Preconditions**:

- MCP server with ID exists in catalog
- Server has a tool with known name

**Test Steps**:

1. Send GET request to `/api/model_catalog/v1alpha1/mcp_servers/{server_id}/tools/{tool_name}`
2. Verify response status is 200 OK
3. Verify response contains McpToolWithServer entity
4. Verify tool details match YAML definition
5. Verify server context is included

**Expected Results**:

- Status: 200 OK
- Response body contains McpToolWithServer with:
  - Tool details: name, description, accessType, parameters
  - Server context: server_id, server_name
- Tool data matches YAML configuration

**Test Data**:

```bash
GET /api/model_catalog/v1alpha1/mcp_servers/123/tools/query
```

**Expected Response**:

```json
{
  "name": "query",
  "description": "Execute PromQL queries",
  "accessType": "read_only",
  "parameters": [
    {
      "name": "query",
      "type": "string",
      "description": "PromQL query expression",
      "required": true
    }
  ],
  "server_id": "123",
  "server_name": "prometheus-mcp"
}
```

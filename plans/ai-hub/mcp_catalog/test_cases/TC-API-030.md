# TC-API-030: List Tools for MCP Server

**Priority**: P1
**Objective**: Verify retrieval of all tools for a specific MCP server with pagination
**Automation Status**: Covered by upstream Go unit tests (`TestFindMCPServerTools` in
`api_mcp_catalog_service_service_test.go`, BFF `TestGetMcpServersTools_Success` in
`mcp_server_catalog_test.go`)

**Preconditions**:

- MCP server with ID exists in catalog
- Server has multiple tools defined

**Test Steps**:

1. Send GET request to `/api/model_catalog/v1alpha1/mcp_servers/{server_id}/tools`
2. Verify response status is 200 OK
3. Verify response contains McpToolsList with tools array
4. Test pagination with pageSize parameter
5. Verify nextPageToken returned when more tools available

**Expected Results**:

- Status: 200 OK
- Response body contains McpToolsList with:
  - tools: Array of tool objects
  - Each tool includes: name, description, accessType, parameters
  - Pagination metadata: nextPageToken (if applicable)
- Tools match those defined in server's YAML configuration

**Test Data**:

```bash
GET /api/model_catalog/v1alpha1/mcp_servers/123/tools
GET /api/model_catalog/v1alpha1/mcp_servers/123/tools?pageSize=5
```

**Expected Response**:

```json
{
  "tools": [
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
      ]
    }
  ],
  "nextPageToken": "token123"
}
```

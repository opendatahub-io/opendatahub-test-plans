# TC-API-001: List All MCP Servers

**Priority**: P0
**Objective**: Verify basic listing of MCP servers from catalog

**Preconditions**:

- Catalog contains at least 5 MCP servers
- YAML catalog files loaded successfully

**Test Steps**:

1. Send GET request to `/api/model_catalog/v1alpha1/mcp_servers`
2. Verify response status is 200 OK
3. Verify response contains `mcpServers` array
4. Verify each server has required fields: id, name, provider, description

**Expected Results**:

- Status: 200 OK
- Response body contains array of MCP servers
- Each server includes: id, name, externalId, provider, description, version, transports, tools, artifacts

**Test Data**:

```bash
GET /api/model_catalog/v1alpha1/mcp_servers
```

**Expected Response**:

```json
{
  "mcpServers": [
    {
      "id": "1",
      "name": "dynatrace-mcp",
      "externalId": "dynatrace-mcp",
      "provider": "Dynatrace",
      "description": "Dynatrace observability MCP server",
      "version": "1.0.1",
      "transports": ["http"],
      "artifacts": [...]
      // Note: tools array excluded by default (includeTools=false)
    },
    ...
  ],
  "nextPageToken": "",
  "pageSize": 10
}
```

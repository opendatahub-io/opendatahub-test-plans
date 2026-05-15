# TC-API-019: List MCP Servers with Tools Included

**Priority**: P1
**Objective**: Verify includeTools parameter includes tools array in response

**Preconditions**:

- Catalog contains servers with defined tools

**Test Steps**:

1. Send GET request with `includeTools=true`
2. Verify response status is 200 OK
3. Verify each server includes tools array
4. Verify each tool has: name, description, parameters, accessType

**Expected Results**:

- Status: 200 OK
- Tools array present in each server
- Each tool has complete metadata

**Test Data**:

```bash
GET /api/model_catalog/v1alpha1/mcp_servers?includeTools=true
```

**Expected Response**:

```json
{
  "mcpServers": [
    {
      "id": "1",
      "name": "dynatrace-mcp",
      "tools": [
        {
          "name": "execute_dql",
          "description": "Execute DQL queries",
          "parameters": {...},
          "accessType": "read_only"
        }
      ],
      ...
    }
  ]
}
```

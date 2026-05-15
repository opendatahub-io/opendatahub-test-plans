# TC-API-018: Verify Remote Server Fields

**Priority**: P1
**Objective**: Verify remote MCP servers have correct endpoint fields

**Preconditions**:

- Catalog contains remote HTTP/SSE servers

**Test Steps**:

1. Get remote server with HTTP endpoint
2. Verify `deploymentMode` = "remote"
3. Verify `endpoints.http` present
4. Verify no OCI artifacts
5. Get remote server with SSE endpoint
6. Verify `endpoints.sse` present

**Expected Results**:

- Remote servers have deploymentMode = "remote"
- HTTP servers have `http` in endpoints
- SSE servers have `sse` in endpoints
- Remote servers do not have OCI artifacts

**Test Data**:

```bash
GET /api/model_catalog/v1alpha1/mcp_servers/{remote_server_id}
```

**Expected Response**:

```json
{
  "deploymentMode": "remote",
  "transports": ["http"],
  "endpoints": {
    "http": "https://mcp.cloudprovider.com/api"
  },
  "artifacts": []
}
```

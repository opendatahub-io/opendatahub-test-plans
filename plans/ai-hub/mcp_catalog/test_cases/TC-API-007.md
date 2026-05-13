# TC-API-007: Filter MCP Servers by Deployment Mode

**Priority**: P1
**Objective**: Verify enum filtering for deploymentMode

**Preconditions**:

- Catalog contains both local and remote servers

**Test Steps**:

1. Send GET request with `filterQuery=deploymentMode='local'`
2. Verify all returned servers have deploymentMode = "local"
3. Send GET request with `filterQuery=deploymentMode='remote'`
4. Verify all returned servers have deploymentMode = "remote"

**Expected Results**:

- Local filter returns only OCI-based servers
- Remote filter returns only HTTP/SSE endpoint servers
- Enum values validated correctly

**Test Data**:

```bash
GET /api/model_catalog/v1alpha1/mcp_servers?filterQuery=deploymentMode='local'
GET /api/model_catalog/v1alpha1/mcp_servers?filterQuery=deploymentMode='remote'
```

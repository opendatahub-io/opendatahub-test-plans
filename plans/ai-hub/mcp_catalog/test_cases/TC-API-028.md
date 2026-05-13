# TC-API-028: Filter Catalog Sources by Asset Type (MCP)

**Priority**: P0
**Objective**: Verify filtering sources by assetType=mcp_servers

**Test Steps**:

1. Send GET request to `/api/model_catalog/v1alpha1/sources?assetType=mcp_servers`
2. Verify response contains only MCP sources
3. Verify model sources excluded

**Expected Results**:

- Only sources with assetType = "mcp_servers" returned
- Model sources not included

**Test Data**:

```bash
GET /api/model_catalog/v1alpha1/sources?assetType=mcp_servers
```

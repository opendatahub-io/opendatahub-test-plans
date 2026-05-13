# TC-API-034: Filter Catalog Labels by Asset Type (MCP)

**Priority**: P0
**Objective**: Verify filtering labels by assetType=mcp_servers

**Preconditions**:

- Catalog has labels configured with `assetType: mcp_servers`

**Test Steps**:

1. Send GET request to `/api/model_catalog/v1alpha1/labels?assetType=mcp_servers`
2. Verify response status is 200 OK
3. Verify response contains only MCP server labels
4. Verify model labels are excluded

**Expected Results**:

- Status: 200 OK
- Only labels with `assetType=mcp_servers` returned
- Model labels not included (including labels without explicit `assetType`)

**Test Data**:

```bash
GET /api/model_catalog/v1alpha1/labels?assetType=mcp_servers
```

**Notes**:

- Mirrors the `/sources` endpoint behavior (TC-API-028)
- Labels without `assetType` in the ConfigMap default to `models` and should not appear

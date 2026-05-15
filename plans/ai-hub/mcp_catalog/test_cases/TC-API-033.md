# TC-API-033: List Catalog Labels (Default Behavior)

**Priority**: P0
**Objective**: Verify that a GET request to `/labels` without an `assetType` parameter returns only
model labels (backward compatibility)

**Preconditions**:

- Catalog has labels configured for both `models` and `mcp_servers` asset types
- Default labels without `assetType` exist (should be treated as `models`)

**Test Steps**:

1. Send GET request to `/api/model_catalog/v1alpha1/labels` (no `assetType` parameter)
2. Verify response status is 200 OK
3. Verify response contains only model labels (including labels without explicit `assetType`)
4. Verify MCP server labels are not included in the response

**Expected Results**:

- Status: 200 OK
- Labels array contains only labels with `assetType=models` or no `assetType` (defaults to `models`)
- Labels with `assetType=mcp_servers` are excluded

**Test Data**:

```bash
GET /api/model_catalog/v1alpha1/labels
```

**Notes**:

- When no `assetType` is specified, the endpoint defaults to returning only model labels (`assetType=models`)
- Labels without an explicit `assetType` in the ConfigMap default to `models` server-side
- To retrieve MCP server labels, use `assetType=mcp_servers` (see TC-API-034)
- To retrieve model labels explicitly, use `assetType=models` (see TC-API-035)
- Mirrors the `/sources` endpoint behavior (TC-API-027)

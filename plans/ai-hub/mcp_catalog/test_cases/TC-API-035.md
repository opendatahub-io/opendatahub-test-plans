# TC-API-035: Filter Catalog Labels by Asset Type (Models)

**Priority**: P1
**Objective**: Verify filtering labels by assetType=models returns the same result as default (no parameter)

**Preconditions**:

- Catalog has labels configured for both `models` and `mcp_servers` asset types

**Test Steps**:

1. Send GET request to `/api/model_catalog/v1alpha1/labels?assetType=models`
2. Verify response status is 200 OK
3. Verify response contains only model labels
4. Verify MCP server labels are excluded
5. Verify result is identical to GET `/labels` without `assetType` parameter (TC-API-033)

**Expected Results**:

- Status: 200 OK
- Only labels with `assetType=models` or no `assetType` (defaults to models) returned
- MCP server labels not included
- Result matches default behavior (TC-API-033)

**Test Data**:

```bash
GET /api/model_catalog/v1alpha1/labels?assetType=models
```

**Notes**:

- Mirrors the `/sources` endpoint behavior (TC-API-029)

# TC-API-029: Filter Catalog Sources by Asset Type (Models)

**Priority**: P1
**Objective**: Verify filtering sources by assetType=models

**Test Steps**:

1. Send GET request to `/api/model_catalog/v1alpha1/sources?assetType=models`
2. Verify response contains only model sources
3. Verify MCP sources excluded

**Expected Results**:

- Only sources with assetType = "models" returned
- MCP sources not included

**Test Data**:

```bash
GET /api/model_catalog/v1alpha1/sources?assetType=models
```

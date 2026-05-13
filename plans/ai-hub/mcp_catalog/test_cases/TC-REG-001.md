# TC-REG-001: Model Catalog Still Works (Non-Regression)

**Priority**: P0
**Objective**: Verify existing Model Catalog functionality not broken by MCP changes

**Test Steps**:

1. Query model catalog sources: `GET /api/model_catalog/v1alpha1/sources?assetType=models`
2. Verify model sources still work
3. List models if applicable
4. Verify no regression in existing API

**Expected Results**:

- Model catalog functionality unchanged
- Sources API supports both asset types
- No breaking changes to existing model endpoints

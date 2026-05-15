# TC-API-013: Combined Search and Filter

**Priority**: P1
**Objective**: Verify keyword search + filter combination (AND logic)

**Test Steps**:

1. Send GET request with `q=github&filterQuery=verifiedSource=true`
2. Verify results match both keyword AND filter
3. Verify AND logic between search and filter

**Expected Results**:

- Only verified servers with "github" keyword returned
- Search and filter combined with AND

**Test Data**:

```bash
GET /api/model_catalog/v1alpha1/mcp_servers?q=github&filterQuery=verifiedSource=true
```

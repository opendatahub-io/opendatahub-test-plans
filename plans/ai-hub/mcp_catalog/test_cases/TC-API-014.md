# TC-API-014: Ordering and Sorting

**Priority**: P1
**Objective**: Verify result ordering

**Test Steps**:

1. Send GET request with `orderBy=name&sortOrder=ASC`
2. Verify results sorted alphabetically by name ascending
3. Send GET request with `orderBy=name&sortOrder=DESC`
4. Verify results sorted alphabetically by name descending

**Expected Results**:

- ASC order: A-Z by name
- DESC order: Z-A by name
- Ordering applied correctly

**Test Data**:

```bash
GET /api/model_catalog/v1alpha1/mcp_servers?orderBy=name&sortOrder=ASC
GET /api/model_catalog/v1alpha1/mcp_servers?orderBy=name&sortOrder=DESC
```

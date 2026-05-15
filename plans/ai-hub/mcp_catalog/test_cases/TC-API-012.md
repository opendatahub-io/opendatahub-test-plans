# TC-API-012: Keyword Search (q parameter)

**Priority**: P1
**Objective**: Verify free-form keyword search

**Test Steps**:

1. Send GET request with `q=monitoring`
2. Verify results include servers with "monitoring" in name, description, or provider
3. Verify case-insensitive matching
4. Verify partial string matching (contains)

**Expected Results**:

- Servers with keyword in name, description, or provider returned
- Case-insensitive search works
- Partial matches included

**Test Data**:

```bash
GET /api/model_catalog/v1alpha1/mcp_servers?q=monitoring
```

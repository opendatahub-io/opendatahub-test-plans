# TC-ERROR-001: Invalid Filter Query Syntax

**Priority**: P1
**Objective**: Verify error handling for malformed filter queries

**Test Steps**:

1. Send GET request with invalid filterQuery: `filterQuery=invalid syntax here`
2. Verify response status is 400 Bad Request
3. Verify error message describes syntax error

**Expected Results**:

- Status: 400 Bad Request
- Error message: "Invalid filter query syntax: ..."

**Test Data**:

```bash
GET /api/model_catalog/v1alpha1/mcp_servers?filterQuery=invalid syntax
```

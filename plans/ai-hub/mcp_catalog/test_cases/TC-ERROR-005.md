# TC-ERROR-005: Unknown Named Query

**Priority**: P1
**Objective**: Verify error for non-existent named query

**Test Steps**:

1. Send GET request with `namedQuery=nonExistentQuery`
2. Verify response status is 400 Bad Request
3. Verify error message lists available named queries

**Expected Results**:

- Status: 400 Bad Request
- Error: "Unknown named query: nonExistentQuery. Available queries: production_ready, security_focused"

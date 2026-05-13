# TC-ERROR-002: Invalid Field Name in Filter

**Priority**: P1
**Objective**: Verify error for non-existent filter field

**Test Steps**:

1. Send GET request with `filterQuery=nonExistentField='value'`
2. Verify response status is 400 Bad Request
3. Verify error message indicates unknown field

**Expected Results**:

- Status: 400 Bad Request
- Error: "Unknown filter field: nonExistentField"

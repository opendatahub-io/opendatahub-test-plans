# TC-ERROR-004: Invalid Page Size (Out of Range)

**Priority**: P2
**Objective**: Verify validation of pageSize parameter

**Test Steps**:

1. Send GET request with `pageSize=-1`
2. Verify error response
3. Send GET request with `pageSize=10000` (too large)
4. Verify error or capped to max

**Expected Results**:

- Negative pageSize rejected with 400 error
- Excessive pageSize capped to maximum (e.g., 100)

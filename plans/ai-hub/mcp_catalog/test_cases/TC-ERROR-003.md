# TC-ERROR-003: Invalid Page Token

**Priority**: P1
**Objective**: Verify error handling for invalid pagination token

**Test Steps**:

1. Send GET request with `pageToken=invalid_token_12345`
2. Verify response status is 400 Bad Request
3. Verify error message about invalid token

**Expected Results**:

- Status: 400 Bad Request
- Error: "Invalid page token"

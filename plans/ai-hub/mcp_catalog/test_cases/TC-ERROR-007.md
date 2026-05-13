# TC-ERROR-007: Special Characters in Search Query

**Priority**: P2
**Objective**: Verify handling of special characters in search

**Test Steps**:

1. Send GET request with `q=test@#$%^&*()`
2. Verify no errors or SQL injection
3. Verify safe handling

**Expected Results**:

- No errors
- Special characters escaped properly
- No SQL injection vulnerability

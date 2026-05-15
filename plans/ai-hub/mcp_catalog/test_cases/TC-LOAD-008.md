# TC-LOAD-008: Missing Required Fields in YAML

**Priority**: P1
**Objective**: Verify validation of required fields

**Test Steps**:

1. Create YAML with server missing required field (e.g., name)
2. Load catalog
3. Verify server rejected with clear error
4. Verify other valid servers loaded

**Expected Results**:

- Server with missing required field rejected
- Error message indicates which field is missing
- Other servers load successfully

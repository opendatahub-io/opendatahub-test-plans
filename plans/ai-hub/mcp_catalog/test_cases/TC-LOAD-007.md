# TC-LOAD-007: Invalid YAML Handling

**Priority**: P1
**Objective**: Verify graceful error handling for invalid YAML

**Test Steps**:

1. Configure catalog source with invalid YAML file (malformed syntax)
2. Start catalog service
3. Verify service starts (degraded mode)
4. Check logs for error message
5. Verify other valid sources still loaded

**Expected Results**:

- Service starts without crashing
- Error logged with file path and line number
- Valid sources still functional
- Invalid source marked as failed

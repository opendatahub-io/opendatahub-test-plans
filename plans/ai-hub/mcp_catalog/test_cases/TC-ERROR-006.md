# TC-ERROR-006: Empty Catalog (No Servers)

**Priority**: P2
**Objective**: Verify behavior when catalog is empty

**Test Steps**:

1. Configure empty catalog (no YAML sources or empty files)
2. Send GET request to list servers
3. Verify response is valid but empty

**Expected Results**:

- Status: 200 OK
- Response: `{"mcpServers": [], "nextPageToken": "", "pageSize": 10}`
- No errors, graceful empty result

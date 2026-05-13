# TC-API-020: List MCP Servers with Tools Excluded (Default)

**Priority**: P1
**Objective**: Verify tools excluded by default when includeTools not specified

**Test Steps**:

1. Send GET request without includeTools parameter
2. Verify response status is 200 OK
3. Verify servers do NOT include tools array

**Expected Results**:

- Status: 200 OK
- Tools array absent from server responses
- Response is smaller/faster without tools

**Test Data**:

```bash
GET /api/model_catalog/v1alpha1/mcp_servers
```

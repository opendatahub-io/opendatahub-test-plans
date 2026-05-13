# TC-API-002: List MCP Servers with Pagination

**Priority**: P0
**Objective**: Verify pagination works correctly

**Preconditions**:

- Catalog contains at least 15 MCP servers

**Test Steps**:

1. Send GET request with `pageSize=5`
2. Verify response contains 5 servers and `nextPageToken`
3. Send second request with `pageToken` from previous response
4. Verify response contains next 5 servers
5. Continue until `nextPageToken` is empty

**Expected Results**:

- First page returns 5 servers with nextPageToken
- Subsequent pages return correct servers
- Last page has empty nextPageToken
- No duplicate servers across pages

**Test Data**:

```bash
GET /api/model_catalog/v1alpha1/mcp_servers?pageSize=5
GET /api/model_catalog/v1alpha1/mcp_servers?pageSize=5&pageToken=<token>
```

# TC-API-032: Pagination Combined with Filter Query

**Priority**: P1
**Objective**: Verify pagination works correctly when combined with filterQuery

**Preconditions**:

- Catalog contains at least 2 MCP servers matching the filter criteria

**Test Steps**:

1. Send GET request with `filterQuery=license='MIT'` and `pageSize=1`
2. Verify response contains exactly 1 server and `nextPageToken`
3. Send second request with same `filterQuery` and `nextPageToken` from previous response
4. Verify response contains exactly 1 server
5. Verify no duplicate servers across pages
6. Verify all returned servers match the filter criteria (license = MIT)

**Expected Results**:

- First page returns 1 server with nextPageToken
- Second page returns 1 different server
- No duplicate servers across pages
- All returned servers have license = MIT
- Total collected servers match expected count for the filter

**Test Data**:

```bash
GET /api/mcp_catalog/v1alpha1/mcp_servers?filterQuery=license='MIT'&pageSize=1
GET /api/mcp_catalog/v1alpha1/mcp_servers?filterQuery=license='MIT'&pageSize=1&nextPageToken=<token>
```

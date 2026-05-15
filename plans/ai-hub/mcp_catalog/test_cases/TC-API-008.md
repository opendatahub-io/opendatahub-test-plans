# TC-API-008: Filter MCP Servers by Transport Type

**Priority**: P1
**Objective**: Verify filtering by transport type array

**Preconditions**:

- Servers support various transports: stdio, http, sse

**Test Steps**:

1. Send GET request with `filterQuery=transports='http'`
2. Verify all returned servers support HTTP transport
3. Verify servers without HTTP excluded

**Expected Results**:

- Only servers with HTTP transport returned
- Equality operator matches against array elements

**Test Data**:

```bash
GET /api/model_catalog/v1alpha1/mcp_servers?filterQuery=transports='http'
```

# TC-API-005: Filter MCP Servers by Tags (CONTAINS operator)

**Priority**: P1
**Objective**: Verify array filtering with CONTAINS

**Preconditions**:

- Servers have tags in customProperties: observability, monitoring, security

**Test Steps**:

1. Send GET request with `filterQuery=tags CONTAINS 'observability'`
2. Verify all returned servers have 'observability' tag
3. Verify count matches expected servers

**Expected Results**:

- Only servers tagged with 'observability' returned
- Servers without tag excluded

**Test Data**:

```bash
GET /api/model_catalog/v1alpha1/mcp_servers?filterQuery=tags CONTAINS 'observability'
```

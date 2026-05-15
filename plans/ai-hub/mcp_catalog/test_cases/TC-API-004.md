# TC-API-004: Filter MCP Servers by Multiple Providers (IN operator)

**Priority**: P1
**Objective**: Verify IN operator for multiple provider selection

**Test Steps**:

1. Send GET request with `filterQuery=provider IN ['Dynatrace', 'CNCF', 'GitHub']`
2. Verify all returned servers have provider in the list
3. Verify servers from other providers excluded

**Expected Results**:

- Only servers with provider = Dynatrace, CNCF, or GitHub returned
- Complete list of matching servers

**Test Data**:

```bash
GET /api/model_catalog/v1alpha1/mcp_servers?filterQuery=provider IN ['Dynatrace', 'CNCF', 'GitHub']
```

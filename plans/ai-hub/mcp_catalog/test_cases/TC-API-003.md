# TC-API-003: Filter MCP Servers by Provider

**Priority**: P0
**Objective**: Verify filtering by provider field

**Preconditions**:

- Catalog contains servers from multiple providers (Dynatrace, CNCF, GitHub)

**Test Steps**:

1. Send GET request with `filterQuery=provider='Dynatrace'`
2. Verify response status is 200 OK
3. Verify all returned servers have provider = "Dynatrace"
4. Verify servers from other providers are not included

**Expected Results**:

- Only Dynatrace servers returned
- Response count matches expected Dynatrace servers
- No servers from other providers

**Test Data**:

```bash
GET /api/model_catalog/v1alpha1/mcp_servers?filterQuery=provider='Dynatrace'
```

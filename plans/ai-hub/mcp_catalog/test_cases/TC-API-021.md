# TC-API-021: Tool Limit Parameter

**Priority**: P1
**Objective**: Verify toolLimit parameter limits number of tools returned

**Preconditions**:

- Server has more than 5 tools

**Test Steps**:

1. Send GET request with `includeTools=true&toolLimit=5`
2. Verify response includes exactly 5 tools per server
3. Send request with `toolLimit=100` (max)
4. Verify limit respected

**Expected Results**:

- toolLimit=5 returns maximum 5 tools
- toolLimit=100 returns maximum 100 tools
- Default toolLimit=10 when includeTools=true

**Test Data**:

```bash
GET /api/model_catalog/v1alpha1/mcp_servers?includeTools=true&toolLimit=5
GET /api/model_catalog/v1alpha1/mcp_servers?includeTools=true&toolLimit=100
```

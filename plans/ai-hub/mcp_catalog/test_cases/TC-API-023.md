# TC-API-023: Invalid toolLimit Parameter Validation

**Priority**: P2
**Objective**: Verify validation of toolLimit parameter edge cases

**Test Steps**:

1. Send GET request with `toolLimit=-1` (negative)
2. Verify response status is 400 Bad Request
3. Send GET request with `toolLimit=0`
4. Verify appropriate handling (error or empty tools)
5. Send GET request with `toolLimit=101` (exceeds max)
6. Verify toolLimit capped at 100

**Expected Results**:

- Negative toolLimit returns 400 error
- toolLimit=0 either returns error or empty tools array
- toolLimit > 100 capped at maximum 100
- Clear error messages for invalid values

**Test Data**:

```bash
GET /api/model_catalog/v1alpha1/mcp_servers?includeTools=true&toolLimit=-1
GET /api/model_catalog/v1alpha1/mcp_servers?includeTools=true&toolLimit=0
GET /api/model_catalog/v1alpha1/mcp_servers?includeTools=true&toolLimit=101
```

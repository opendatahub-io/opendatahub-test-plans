# TC-API-006: Filter MCP Servers by Security Indicators (Boolean)

**Priority**: P1
**Objective**: Verify boolean filtering for security indicators

**Preconditions**:

- Servers have customProperties with verifiedSource, sast booleans

**Test Steps**:

1. Send GET request with `filterQuery=verifiedSource=true AND sast=true`
2. Verify all returned servers have both indicators = true
3. Verify servers without both indicators excluded

**Expected Results**:

- Only verified servers with SAST scans returned
- Boolean AND logic applied correctly

**Test Data**:

```bash
GET /api/model_catalog/v1alpha1/mcp_servers?filterQuery=verifiedSource=true AND sast=true
```

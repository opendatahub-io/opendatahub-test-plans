# TC-API-010: Complex Filter Query (Multiple Conditions with AND/OR)

**Priority**: P1
**Objective**: Verify complex boolean logic in filters

**Test Steps**:

1. Send GET request with complex filter:
   `filterQuery=provider='CNCF' AND (tags CONTAINS 'security' OR secureEndpoint=true)`
2. Verify all returned servers match criteria
3. Verify boolean logic applied correctly

**Expected Results**:

- Only CNCF servers with security tag OR secure endpoint returned
- Complex AND/OR logic processed correctly

**Test Data**:

```bash
GET /api/model_catalog/v1alpha1/mcp_servers?filterQuery=provider='CNCF' AND (tags CONTAINS 'security' OR secureEndpoint=true)
```

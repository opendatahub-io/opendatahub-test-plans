# TC-API-011: Named Query Execution

**Priority**: P1
**Objective**: Verify pre-defined named queries work

**Preconditions**:

- Named query "production_ready" defined in catalog config

**Test Steps**:

1. Send GET request with `namedQuery=production_ready`
2. Verify response contains only production-ready servers
3. Verify named query criteria applied (e.g., verifiedSource=true, stable version)

**Expected Results**:

- Only servers matching named query criteria returned
- Named query resolved and executed correctly

**Test Data**:

```bash
GET /api/model_catalog/v1alpha1/mcp_servers?namedQuery=production_ready
```

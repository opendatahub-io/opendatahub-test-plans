# TC-SEC-001: Unauthenticated Access Denied

**Priority**: P0
**Objective**: Verify API requires authentication

**Test Steps**:

1. Send GET request to `/api/model_catalog/v1alpha1/mcp_servers` without auth token
2. Verify response status is 401 Unauthorized

**Expected Results**:

- Status: 401 Unauthorized
- Access denied without valid auth

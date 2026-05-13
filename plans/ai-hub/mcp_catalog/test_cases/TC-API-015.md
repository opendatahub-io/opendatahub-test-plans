# TC-API-015: Filter by Name (Exact Match)

**Priority**: P0
**Objective**: Verify filtering by exact server name

**Test Steps**:

1. Send GET request with `name=dynatrace-mcp`
2. Verify response contains only the server with exact name match
3. Verify case handling

**Expected Results**:

- Single server with exact name returned
- Case-insensitive match if applicable

**Test Data**:

```bash
GET /api/model_catalog/v1alpha1/mcp_servers?name=dynatrace-mcp
```

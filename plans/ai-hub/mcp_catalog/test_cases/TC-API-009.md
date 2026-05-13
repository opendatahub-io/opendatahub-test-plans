# TC-API-009: Filter MCP Servers by License (SPDX)

**Priority**: P1
**Objective**: Verify filtering by SPDX license identifier

**Preconditions**:

- Servers have various licenses: apache-2.0, mit, bsd-3-clause, proprietary

**Test Steps**:

1. Send GET request with `filterQuery=license='BSD 3-Clause'`
2. Verify only servers with BSD-3-Clause license are returned
3. Verify license field shows the transformed display name

**Expected Results**:

- Only matching licensed servers returned
- Response shows license as transformed display name (e.g., "BSD 3-Clause")
- Filtering uses the display name, not the raw SPDX ID

**Test Data**:

```bash
GET /api/mcp_catalog/v1alpha1/mcp_servers?filterQuery=license='BSD 3-Clause'
```

**Expected Response**:

```json
{
  "items": [
    {
      "name": "file-manager",
      "license": "BSD 3-Clause"
    }
  ]
}
```

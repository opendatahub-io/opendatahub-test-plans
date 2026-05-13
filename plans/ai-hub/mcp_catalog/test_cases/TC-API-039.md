# TC-API-039: Filter MCP Servers by sourceLabel=null (Unlabeled Sources)

**Priority**: P1
**Objective**: Verify that sourceLabel=null returns only MCP servers from sources without labels

**Preconditions**:

- At least 1 MCP catalog source configured without labels (empty labels list)
- At least 1 MCP catalog source configured with labels

**Test Steps**:

1. Send GET request with `sourceLabel=null`
2. Verify response status is 200 OK
3. Verify response contains only MCP servers from sources that have no labels assigned
4. Verify MCP servers from labeled sources are excluded

**Expected Results**:

- Status: 200 OK
- Only MCP servers from unlabeled sources returned
- Servers from sources with labels are not included

**Test Data**:

```bash
GET /api/model_catalog/v1alpha1/mcp_servers?sourceLabel=null
```

**Notes**:

- "null" is a special string value, not an empty parameter
- Sources with `labels: []` (empty list) are considered unlabeled
- Mirrors the model catalog behavior where `sourceLabel=null` returns models from unlabeled sources

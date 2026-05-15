# TC-API-036: Filter MCP Servers by sourceLabel (Matching Source)

**Priority**: P0
**Objective**: Verify that the sourceLabel query parameter filters MCP servers to only those
from matching sources

**Preconditions**:

- At least 2 MCP catalog sources configured with different labels
- Each source contains different MCP servers

**Test Steps**:

1. Send GET request with `sourceLabel=<label>` matching one configured source
2. Verify response status is 200 OK
3. Verify response contains only MCP servers from the matching source
4. Verify MCP servers from other sources are excluded
5. Verify the `source_id` field on returned servers matches the expected source

**Expected Results**:

- Status: 200 OK
- Only MCP servers belonging to sources with the matching label are returned
- Servers from other sources are not included

**Test Data**:

```bash
GET /api/model_catalog/v1alpha1/mcp_servers?sourceLabel=Test+MCP+Servers
```

**Notes**:

- sourceLabel matching is case-insensitive
- Multiple labels can be comma-separated (OR semantics): `sourceLabel=Label1,Label2`
- This was a bug fix — previously sourceLabel was accepted but silently ignored (RHOAIENG-54108)
- Mirrors the model catalog sourceLabel behavior on `/models` endpoint

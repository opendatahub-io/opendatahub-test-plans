# TC-API-038: List MCP Servers Without sourceLabel Returns All

**Priority**: P1
**Objective**: Verify that omitting the sourceLabel parameter returns all MCP servers from all sources

**Preconditions**:

- At least 2 MCP catalog sources configured with MCP servers

**Test Steps**:

1. Send GET request without sourceLabel parameter
2. Verify response status is 200 OK
3. Verify response contains MCP servers from all configured sources
4. Compare with sum of servers from each individual source (using sourceLabel filter)

**Expected Results**:

- Status: 200 OK
- All MCP servers from all sources returned
- Total count >= sum of individually filtered counts (sources without labels contribute to total)

**Test Data**:

```bash
GET /api/model_catalog/v1alpha1/mcp_servers
```

**Notes**:

- Partially overlaps with TC-API-001 (List All MCP Servers)
- This test specifically validates behavior in context of sourceLabel filtering

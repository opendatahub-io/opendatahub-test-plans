# TC-API-037: Filter MCP Servers by sourceLabel (No Match Returns Empty)

**Priority**: P1
**Objective**: Verify that a sourceLabel matching no configured source returns an empty list

**Preconditions**:

- At least 1 MCP catalog source configured with a label

**Test Steps**:

1. Send GET request with `sourceLabel=nonexistent`
2. Verify response status is 200 OK
3. Verify response contains an empty items list
4. Verify size is 0

**Expected Results**:

- Status: 200 OK
- Empty items list returned
- Size: 0

**Test Data**:

```bash
GET /api/model_catalog/v1alpha1/mcp_servers?sourceLabel=nonexistent
```

**Notes**:

- The endpoint should not return an error for non-matching labels
- This is an optimization — the provider is not called when no sources match

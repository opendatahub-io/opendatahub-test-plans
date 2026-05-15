# TC-API-027: List Catalog Sources (Default Behavior)

**Priority**: P0
**Objective**: Verify that a GET request to `/sources` without an `assetType` parameter returns
only model catalog sources (not MCP sources)

**Preconditions**:

- Catalog has at least 1 model source and 1 MCP source configured

**Test Steps**:

1. Send GET request to `/api/model_catalog/v1alpha1/sources` (no `assetType` parameter)
2. Verify response status is 200 OK
3. Verify response contains only model catalog sources
4. Verify MCP catalog sources are not included in the response

**Expected Results**:

- Status: 200 OK
- Sources array contains only model catalog sources
- MCP catalog sources are excluded (requires `assetType=mcp_servers` to be returned)
- Each source has: id, name, labels

**Test Data**:

```bash
GET /api/model_catalog/v1alpha1/sources
```

**Expected Response**:

```json
{
  "items": [
    {
      "id": "redhat_ai_models",
      "name": "Red Hat AI",
      "labels": ["Red Hat AI"]
    },
    {
      "id": "redhat_ai_validated_models",
      "name": "Red Hat AI validated",
      "labels": ["Red Hat AI validated"]
    }
  ]
}
```

**Notes**:

- When no `assetType` is specified, the endpoint defaults to returning only model sources (`assetType=models`)
- To retrieve MCP sources, use `assetType=mcp_servers` (see TC-API-028)
- To retrieve model sources explicitly, use `assetType=models` (see TC-API-029)

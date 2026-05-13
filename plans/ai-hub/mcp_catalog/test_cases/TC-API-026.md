# TC-API-026: Get Filter Options

**Priority**: P1
**Objective**: Verify filter_options endpoint returns all filterable fields

**Test Steps**:

1. Send GET request to `/api/model_catalog/v1alpha1/mcp_servers/filter_options`
2. Verify response status is 200 OK
3. Verify response contains all filterable properties
4. Verify each property has correct type (string, array, boolean, enum)

**Expected Results**:

- Status: 200 OK
- Filterable properties include:
  - Entity: id, name, externalId, createTimeSinceEpoch, lastUpdateTimeSinceEpoch
  - Core: source_id, description, provider, license, license_link, logo, readme
  - Arrays: tags, transports
  - Enum: deploymentMode
  - Booleans: verifiedSource, secureEndpoint, sast, readOnlyTools

**Test Data**:

```bash
GET /api/model_catalog/v1alpha1/mcp_servers/filter_options
```

**Expected Response**:

```json
{
  "filterableProperties": [
    { "name": "id", "type": "string" },
    { "name": "name", "type": "string" },
    { "name": "provider", "type": "string" },
    { "name": "tags", "type": "array" },
    { "name": "transports", "type": "array" },
    { "name": "deploymentMode", "type": "enum", "values": ["local", "remote"] },
    { "name": "verifiedSource", "type": "boolean" },
    { "name": "sast", "type": "boolean" },
    { "name": "secureEndpoint", "type": "boolean" },
    { "name": "readOnlyTools", "type": "boolean" }
  ]
}
```

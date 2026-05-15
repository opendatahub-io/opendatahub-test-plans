# TC-API-016: Get MCP Server by ID

**Priority**: P0
**Objective**: Verify retrieval of single MCP server by ID

**Preconditions**:

- MCP server with known ID exists in catalog

**Test Steps**:

1. Send GET request to `/api/model_catalog/v1alpha1/mcp_servers/{id}`
2. Verify response status is 200 OK
3. Verify response contains complete server details
4. Verify all required fields present

**Expected Results**:

- Status: 200 OK
- Server details include: id, name, provider, description, version, transports, tools[],
  artifacts[], endpoints, customProperties, license, licenseLink, logo, readme

**Test Data**:

```bash
GET /api/model_catalog/v1alpha1/mcp_servers/1
```

**Expected Response**:

```json
{
  "id": "1",
  "name": "dynatrace-mcp",
  "externalId": "dynatrace-mcp",
  "provider": "Dynatrace",
  "description": "Dynatrace observability MCP server",
  "version": "1.0.1",
  "transports": ["http"],
  "deploymentMode": "local",
  "artifacts": [
    {
      "uri": "oci://ghcr.io/dynatrace-oss/dynatrace-mcp-server:1.0.1",
      "artifactType": "container_image"
    }
  ],
  "customProperties": {
    "verifiedSource": {
      "metadataType": "MetadataBoolValue",
      "bool_value": true
    },
    "observability": {
      "metadataType": "MetadataStringValue",
      "string_value": ""
    }
  },
  "license": "Apache 2.0",
  "license_link": "https://github.com/dynatrace-oss/dynatrace-mcp-server/blob/main/LICENSE"
  // Note: tools array excluded by default - use includeTools=true to include
}
```

# TC-LOAD-001: Load MCP Servers from YAML

**Priority**: P0
**Objective**: Verify YAML catalog files are loaded into database on startup

**Preconditions**:

- YAML catalog source configured in dev-mcp-catalog-sources.yaml
- MCP server definitions in organization-mcp-servers.yaml

**Test Steps**:

1. Configure catalog source with yamlCatalogPath
2. Restart catalog service
3. Query database directly to verify data loaded
4. Send GET request to list MCP servers via API
5. Verify servers from YAML appear in both database and API response
6. Verify database data matches API response
7. Verify all fields loaded correctly

**Expected Results**:

- Servers from YAML file loaded into database
- Database records match YAML source data
- API response matches database records
- All properties preserved (name, provider, tools, artifacts, customProperties)
- Source_id set to catalog source ID

**Validation**:

- Query database: `SELECT * FROM mcp_servers WHERE source_id = '<catalog_source_id>'`
- Query API: `GET /api/model_catalog/v1alpha1/mcp_servers`
- Verify server count matches YAML file in both database and API
- Verify specific server details match YAML definition
- Verify database fields match API response fields (id, name, externalId, provider, etc.)

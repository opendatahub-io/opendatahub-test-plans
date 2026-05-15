# TC-API-024: Documentation Fields Validation

**Priority**: P1
**Objective**: Verify documentation-related fields are stored and retrieved correctly
**Coverage**: Covered upstream — `test_mcp_server_providers()` validates provider matches YAML,
`test_mcp_servers_loaded()` checks required fields (id, name, provider, description).
Documentation fields (readme, documentationUrl, repositoryUrl, sourceCode) are optional
(`omitempty` in Go struct).

**Preconditions**:

- Server has documentation fields defined in YAML

**Test Steps**:

1. Load server with readme, documentationUrl, repositoryUrl, sourceCode fields
2. Get server via API
3. Verify all documentation fields present
4. Verify readme contains Markdown content
5. Verify URLs are valid format

**Expected Results**:

- readme field contains full Markdown text
- documentationUrl, repositoryUrl, sourceCode fields present
- publishedDate and lastUpdated timestamps present
- URLs are valid HTTP/HTTPS format

**Test Data**:

```bash
GET /api/model_catalog/v1alpha1/mcp_servers/{id}
```

**Expected Response**:

```json
{
  "id": "1",
  "name": "example-mcp",
  "readme": "# Example MCP Server\n\nFull documentation...",
  "documentationUrl": "https://example.com/docs",
  "repositoryUrl": "https://github.com/example/mcp",
  "sourceCode": "example/mcp",
  "publishedDate": "2025-01-10",
  "lastUpdated": "2025-01-15"
}
```

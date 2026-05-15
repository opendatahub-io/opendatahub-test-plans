# TC-DATA-004: Verify Remote Endpoint URL Validation

**Priority**: P1
**Objective**: Verify HTTP/SSE endpoint URLs validated

**Automation Status**: Covered by upstream Go unit tests (`TestYamlMCPEndpointsValidation` in `providers_test.go`)

**Test Steps**:

1. Attempt to load remote server with invalid URL (not HTTP/HTTPS)
2. Verify error
3. Load remote server with valid HTTPS URL
4. Verify endpoint stored

**Expected Results**:

- Invalid URLs rejected: `ftp://`, `file://`, malformed URLs
- Valid URLs accepted: `https://api.example.com/mcp`, `https://stream.example.com/events`

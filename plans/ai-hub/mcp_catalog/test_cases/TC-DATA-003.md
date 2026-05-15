# TC-DATA-003: Verify Artifact URIs Validated

**Priority**: P1
**Objective**: Verify artifact URIs are valid on load
**Coverage**: Upstream (API contract test, no full cluster needed)
**Bug**: RHOAIENG-54555 — URI validation is currently missing for both MCP servers and model
catalog sources

**Test Steps**:

1. Attempt to load server with invalid OCI URI (malformed)
2. Verify error or warning logged
3. Load server with valid OCI URI
4. Verify artifact stored correctly

**Expected Results**:

- Invalid URIs rejected or flagged
- Valid URIs accepted: `oci://ghcr.io/org/repo:tag`, `npm:@modelcontextprotocol/server-name`

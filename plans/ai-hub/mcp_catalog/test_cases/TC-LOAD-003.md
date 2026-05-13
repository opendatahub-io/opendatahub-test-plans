# TC-LOAD-003: YAML Loading with Glob Filtering (Include)

**Priority**: P1
**Objective**: Verify includedServers glob patterns filter loaded servers

**Preconditions**:

- Catalog source configured with `includedServers: ["github-*", "slack-*"]`
- YAML file contains servers matching and not matching patterns

**Test Steps**:

1. Configure source with include patterns
2. Load catalog
3. Verify only servers matching patterns loaded
4. Verify excluded servers not in database

**Expected Results**:

- Only github-*and slack-* servers loaded
- Other servers filtered out
- Glob matching is case-insensitive

**Test Data**:

```yaml
catalogs:
  - id: org_mcp_servers
    includedServers: ["github-*", "slack-*"]
```

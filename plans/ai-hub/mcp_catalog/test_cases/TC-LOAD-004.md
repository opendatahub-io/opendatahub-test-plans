# TC-LOAD-004: YAML Loading with Glob Filtering (Exclude)

**Priority**: P1
**Objective**: Verify excludedServers glob patterns prevent loading

**Preconditions**:

- Catalog source configured with `excludedServers: ["*-alpha", "*-deprecated"]`
- YAML file contains alpha/deprecated servers

**Test Steps**:

1. Configure source with exclude patterns
2. Load catalog
3. Verify alpha and deprecated servers NOT loaded
4. Verify other servers loaded

**Expected Results**:

- Servers matching exclude patterns filtered out
- Non-matching servers loaded successfully
- Exclusions take precedence over inclusions

**Test Data**:

```yaml
catalogs:
  - id: org_mcp_servers
    excludedServers: ["*-alpha", "*-deprecated"]
```

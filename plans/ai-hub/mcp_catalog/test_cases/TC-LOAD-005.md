# TC-LOAD-005: YAML Loading with Include and Exclude Combined

**Priority**: P1
**Objective**: Verify include and exclude patterns work together (exclusion wins)

**Test Steps**:

1. Configure source with:
   - `includedServers: ["github-*"]`
   - `excludedServers: ["*-alpha"]`
2. Add server "github-actions-alpha" to YAML
3. Load catalog
4. Verify "github-actions-alpha" NOT loaded (excluded)
5. Verify "github-mcp" loaded (included, not excluded)

**Expected Results**:

- Exclusions take precedence
- Server matching both include and exclude is filtered out

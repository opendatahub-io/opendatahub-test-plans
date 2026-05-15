# TC-LOAD-014: Named Query Merging from Multiple Config Files

**Priority**: P1
**Objective**: Verify named queries merged from multiple configuration files
**Automation Status**: Covered by upstream Go unit tests — no downstream automation needed

**Coverage Note**:
Named query merging follows the same field-level merge logic as MCP sources
(see TC-LOAD-006). The merge function is covered by upstream unit tests in
`catalog/internal/catalog/mcp_source_merge_test.go`.

Named query **execution** (not merging) is covered by downstream tests in
PR [#1216](https://github.com/opendatahub-io/opendatahub-tests/pull/1216)
(`test_named_queries.py` — RHOAIENG-52375).

No additional downstream merge test is needed because:

1. The merge logic is the same as TC-LOAD-006 (upstream unit-tested)
2. Named query execution is already tested downstream
3. The merge integration layer is minimal

**Original Test Steps** (for reference):

1. Create base config with named query:

   ```yaml
   namedQueries:
     production_ready:
       verifiedSource: { operator: "=", value: true }
   ```

2. Create override config with same query name:

   ```yaml
   namedQueries:
     production_ready:
       verifiedSource: { operator: "=", value: true }
       sast: { operator: "=", value: true }
   ```

3. Load catalog
4. Execute named query via API
5. Verify override query used (includes both conditions)

**Expected Results**:

- Later config files override earlier named queries
- Named query from override file used
- Query executes with merged conditions
- No errors or conflicts

**Test Data**:

```bash
GET /api/model_catalog/v1alpha1/mcp_servers?namedQuery=production_ready
```

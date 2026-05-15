# TC-LOAD-006: Field-Level Merging from Multiple Config Files

**Priority**: P1
**Objective**: Verify field-level merging when same source defined in multiple files
**Automation Status**: Covered by upstream Go unit tests — no downstream automation needed

**Coverage Note**:
This test case is fully covered by upstream unit tests in
`catalog/internal/catalog/mcp_source_merge_test.go` (~383 lines), which validate:

- Single field overrides
- Complete and partial overrides at each level
- Empty string `""` preserves base values
- Empty slice `[]` clears base values
- Origin field tracking
- 3+ file cascading (community → org → team)

No downstream integration test is needed because:

1. The merge logic is thoroughly unit-tested in Go
2. The loader integration is minimal (~20 lines in `mcp_loader.go`)
3. There is no API endpoint to read back source-level fields (e.g., labels, Origin), making
   end-to-end verification of merge results not possible through the REST API

**Original Test Steps** (for reference):

1. Define source in community-sources.yaml:

   ```yaml
   catalogs:
     - id: mcp_community
       enabled: true
       labels: ["Community"]
   ```

2. Override in org-sources.yaml:

   ```yaml
   catalogs:
     - id: mcp_community
       labels: ["Validated", "Enterprise"]
       includedServers: ["github-*"]
   ```

3. Load catalog
4. Verify merged result:
   - labels = ["Validated", "Enterprise"] (overridden)
   - enabled = true (preserved from base)
   - includedServers = ["github-*"] (added)

**Expected Results**:

- Later config files override earlier ones (field-level)
- Fields not in override preserved from base
- Merging is field-level, not source-level replacement

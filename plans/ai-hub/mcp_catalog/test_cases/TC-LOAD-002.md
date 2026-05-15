# TC-LOAD-002: YAML Loading with Multiple Sources

**Priority**: P1
**Objective**: Verify multiple YAML sources can be loaded

**Preconditions**:

- 2+ catalog sources configured with different YAML files

**Test Steps**:

1. Configure multiple catalog sources
2. Restart catalog service
3. Verify servers from all sources loaded
4. Verify servers tagged with correct source_id

**Expected Results**:

- Servers from all sources present in database
- source_id correctly set for each server
- No conflicts between sources

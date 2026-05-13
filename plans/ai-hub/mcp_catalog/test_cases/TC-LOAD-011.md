# TC-LOAD-011: Source Cleanup on Disable

**Priority**: P1
**Objective**: Verify servers are deleted when source is disabled

**Preconditions**:

- Catalog source with servers loaded
- Source marked as enabled=true

**Test Steps**:

1. Load catalog with enabled source
2. Verify servers from source present in database
3. Update source configuration to enabled=false
4. Reload catalog
5. Query database for servers from that source
6. Verify servers deleted

**Expected Results**:

- Servers from disabled source removed from database
- Servers from other sources remain
- No orphaned data

**Test Data**:

```yaml
# Before: enabled source
catalogs:
  - id: test_source
    enabled: true

# After: disabled source
catalogs:
  - id: test_source
    enabled: false
```

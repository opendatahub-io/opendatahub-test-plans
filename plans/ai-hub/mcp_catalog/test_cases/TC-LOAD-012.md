# TC-LOAD-012: Source Cleanup on Removal

**Priority**: P1
**Objective**: Verify servers are deleted when source removed from config

**Preconditions**:

- Catalog source with servers loaded

**Test Steps**:

1. Load catalog with source A and source B
2. Verify servers from both sources in database
3. Remove source A from configuration file
4. Reload catalog
5. Verify source A servers deleted
6. Verify source B servers remain

**Expected Results**:

- Servers from removed source deleted from database
- Servers from remaining sources unchanged
- Clean removal without errors

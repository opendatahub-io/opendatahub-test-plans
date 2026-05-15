# TC-LOAD-013: Timestamp Preservation from YAML

**Priority**: P1
**Objective**: Verify timestamps from YAML preserved in database

**Preconditions**:

- YAML file with explicit createTimeSinceEpoch and lastUpdateTimeSinceEpoch

**Test Steps**:

1. Load server with specific timestamps in YAML:

   ```yaml
   createTimeSinceEpoch: "1736510400000"
   lastUpdateTimeSinceEpoch: "1736510400000"
   ```

2. Query server via API
3. Verify timestamps match YAML values exactly

**Expected Results**:

- createTimeSinceEpoch preserved from YAML
- lastUpdateTimeSinceEpoch preserved from YAML
- Timestamps not auto-generated/overwritten
- Historical accuracy maintained

# TC-LOAD-009: SPDX License Transformation on Load

**Priority**: P1
**Objective**: Verify SPDX license IDs are transformed on load via `basecatalog.TransformLicenseToHumanReadable()`

**Automation Status**: Covered by upstream Go unit tests —
`catalog/internal/catalog/mcpcatalog/providers_test.go::TestYamlMCPServerLicenseTransformation`
and `TestYamlMCPServerLicenseConsistencyWithModels`. No downstream automation needed.

**Preconditions**:

- YAML file contains server with `license: apache-2.0`

**Test Steps**:

1. Load catalog
2. Query API: `GET /api/mcp_catalog/v1alpha1/mcp_servers`
3. Verify response shows transformed display name

**Expected Results**:

- SPDX ID is transformed to display name at load time (not on retrieval)
- Transformation uses the same function as model catalog (`basecatalog.TransformLicenseToHumanReadable()`)
- apache-2.0 → "Apache 2.0"

**Test Data**:

```yaml
name: test-server
license: apache-2.0
```

**Expected API Response**:

```json
{
  "license": "Apache 2.0"
}
```

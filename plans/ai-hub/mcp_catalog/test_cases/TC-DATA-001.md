# TC-DATA-001: Verify All SPDX License Transformations

**Priority**: P1
**Objective**: Verify all common SPDX IDs transformed correctly

**Automation Status**: Covered by upstream Go unit tests —
`catalog/internal/catalog/basecatalog/license_transform_test.go` (40+ test cases) and
`catalog/internal/catalog/mcpcatalog/providers_test.go::TestYamlMCPServerLicenseTransformation`
(16 test cases). No downstream automation needed.

**Test Steps**:

1. Load servers with licenses: apache-2.0, mit, bsd-3-clause, gpl-3.0, mpl-2.0, proprietary
2. Query each server via API
3. Verify display names:
   - apache-2.0 → "Apache 2.0"
   - mit → "MIT"
   - bsd-3-clause → "BSD 3-Clause"
   - gpl-3.0 → "GPL 3.0"
   - mpl-2.0 → "MPL 2.0"
   - proprietary → "proprietary" (passthrough, not in lookup table)

**Expected Results**:

- All transformations correct per `basecatalog.TransformLicenseToHumanReadable()`
- Unknown SPDX IDs returned unchanged
- Transformation is case-insensitive on input (e.g., `MIT`, `Mit`, `mit` all produce `MIT`)

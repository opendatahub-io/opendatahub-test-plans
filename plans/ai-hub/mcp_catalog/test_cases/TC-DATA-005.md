# TC-DATA-005: Verify Custom Properties Metadata Types

**Priority**: P1
**Objective**: Verify all MetadataValue types supported
**Coverage**: Covered upstream — Go tests (`openapi_embedmd_converter_util_test.go`) cover all
metadata type conversions (MetadataBoolValue, MetadataIntValue, MetadataDoubleValue,
MetadataStringValue, MetadataStructValue). Python `test_custom_properties_loaded` validates MCP
server custom properties round-trip from YAML.
`test_models_custom_properties_has_valid_structure` validates MetadataStringValue structure for
models.

**Test Steps**:

1. Load server with customProperties containing:
   - MetadataStringValue (tags)
   - MetadataBoolValue (security indicators)
   - MetadataIntValue (numeric)
   - MetadataDoubleValue (floating point)
2. Query via API
3. Verify all types preserved

**Expected Results**:

- All metadata types stored and retrieved correctly
- Type information preserved in response

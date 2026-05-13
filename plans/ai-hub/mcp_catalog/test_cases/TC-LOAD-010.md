# TC-LOAD-010: Custom Properties Loading (Tags and Booleans)

**Priority**: P1
**Objective**: Verify custom properties loaded correctly

**Preconditions**:

- YAML defines customProperties:

  ```yaml
  customProperties:
    observability:
      metadataType: MetadataStringValue
      string_value: ""
    verifiedSource:
      metadataType: MetadataBoolValue
      bool_value: true
  ```

**Test Steps**:

1. Load catalog
2. Query server via API
3. Verify customProperties in response
4. Verify tags stored as MetadataStringValue with empty string
5. Verify booleans stored as MetadataBoolValue

**Expected Results**:

- Tags stored correctly (empty string value)
- Booleans stored correctly (true/false)
- Properties accessible via API and filterable

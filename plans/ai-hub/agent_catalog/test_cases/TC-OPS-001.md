---
test_case_id: TC-OPS-001
source_key: RHOAIENG-70680
priority: P0
status: Draft
automation_status: Not Started
last_updated: "2026-07-07"
upgrade_phase: post
---
# TC-OPS-001: Operator creates default-catalog-sources ConfigMap with agent section

**Objective**: Verify that the RHOAI operator creates the
`default-catalog-sources` ConfigMap containing an `agent_catalogs`
section with Red Hat starter kits.

**Preconditions**:
- RHOAI operator installed with agent catalog feature enabled

**Test Steps**:
1. Verify the `default-catalog-sources` ConfigMap exists in the RHOAI
   namespace
2. Read the ConfigMap data and check for the `agent_catalogs` section
3. Verify the agent_catalogs section references the Red Hat starter kits
   catalog

**Expected Results**:
- ConfigMap `default-catalog-sources` exists in the RHOAI namespace
- ConfigMap data contains an `agent_catalogs` key with at least one
  catalog entry
- The default entry points to the Red Hat starter kits catalog YAML
- The `enabled` field for the default agent source is `true`

**Test Data**:
```bash
oc get configmap default-catalog-sources \
  -n ${RHOAI_NAMESPACE} -o yaml \
  | grep -A 10 "agent_catalogs"
```

**Notes**: To be filled later in the process.

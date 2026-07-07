---
test_case_id: TC-OPS-002
source_key: RHOAIENG-70680
priority: P0
status: Draft
automation_status: Not Started
last_updated: "2026-07-07"
upgrade_phase: post
---
# TC-OPS-002: Operator mounts agent catalog volume to catalog service pod

**Objective**: Verify that the operator configures the correct volume
mounts for agent catalog data in the catalog service deployment.

**Test Steps**:

1. Get the catalog service deployment and inspect its volume mounts
2. Verify the shared-data volume is mounted with the agents-catalog.yaml
3. Verify the `--catalogs-path` argument includes the agent sources path

**Expected Results**:

- The catalog service pod has a volume mount for shared-data at
  the expected path
- The init container writes `agents-catalog.yaml` to the shared-data
  volume
- The container arguments include `--catalogs-path` pointing to the
  agent catalog sources file
- The volume mount is read-only for the main container

**Test Data**:

```bash
oc get deployment model-catalog \
  -n ${RHOAI_NAMESPACE} -o json \
  | jq '.spec.template.spec.containers[0].volumeMounts'

oc get deployment model-catalog \
  -n ${RHOAI_NAMESPACE} -o json \
  | jq '.spec.template.spec.containers[0].args'
```

**Notes**: To be filled later in the process.

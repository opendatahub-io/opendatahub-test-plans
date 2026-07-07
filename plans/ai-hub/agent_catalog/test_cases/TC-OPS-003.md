---
test_case_id: TC-OPS-003
source_key: RHOAIENG-70680
priority: P0
status: Draft
automation_status: Not Started
last_updated: "2026-07-07"
upgrade_phase: post
---
# TC-OPS-003: Agent plugin registers and loads agents on pod startup

**Objective**: Verify that when the catalog service pod starts, the agent
plugin registers successfully and loads agents from the configured
catalog source files.

**Test Steps**:
1. Check the catalog service pod logs for agent plugin registration
   messages
2. Verify `/readyz` returns healthy status including the agent plugin
3. Send GET request to `/api/agent_catalog/v1alpha1/agents` and verify
   agents are loaded

**Expected Results**:
- Pod logs contain messages indicating agent catalog plugin registration
- Pod logs show the number of agents loaded from the catalog YAML
- `/readyz` reports healthy and includes the agent_catalog plugin
- The list agents endpoint returns the expected number of agents (15
  from default Red Hat starter kits)

**Test Data**:
```bash
oc logs deployment/model-catalog \
  -n ${RHOAI_NAMESPACE} \
  | grep -i "agent"
```

**Notes**: To be filled later in the process.

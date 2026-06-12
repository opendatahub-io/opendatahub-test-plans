---
test_case_id: TC-INFRA-002
source_key: RHAISTRAT-1456
priority: P1
status: Draft
automation_status: Not Started
last_updated: "2026-06-12"
---
# TC-INFRA-002: config.yaml Memories and Compaction provider registration

**Objective**: Validate that LlamaStack's config.yaml correctly
registers the Memories API and Compaction API providers and that both
are active after deployment.

**Preconditions**:

- LlamaStack deployed via LlamaStackDistribution CR with config.yaml
  containing Memories and Compaction provider entries
- LlamaStack pod in Running/Ready state on port 8321

**Test Steps**:

1. Deploy LlamaStack with a config.yaml that includes Memories and
   Compaction provider registrations (see TC-INFRA-001 for CR
   example).
2. Verify the config.yaml was mounted correctly in the pod:

   ```bash
   oc exec deploy/llamastack -n <namespace> -- \
     cat /app/config.yaml | grep -A5 "memory\|compaction"
   ```

3. Query LlamaStack's provider listing endpoint to verify both
   providers are registered:

   ```bash
   oc exec deploy/llamastack -n <namespace> -- \
     curl -s http://localhost:8321/providers/list
   ```

4. Check LlamaStack startup logs for provider registration
   confirmations and absence of errors:

   ```bash
   oc logs deploy/llamastack -n <namespace> | \
     grep -i "provider\|memory\|compaction"
   ```

**Expected Results**:

- config.yaml contains entries for both `memory` and `compaction`
  providers
- Provider listing endpoint returns JSON array including both
  provider types with status indicating active/healthy
- LlamaStack startup logs show successful registration of both
  providers (no ERROR or WARNING related to provider init)
- No stack traces or connection failures in logs related to
  provider registration

**Notes**: To be filled later in the process.

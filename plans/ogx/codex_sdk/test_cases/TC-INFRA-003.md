---
test_case_id: TC-INFRA-003
source_key: RHAISTRAT-1456
priority: P2
status: Draft
automation_status: Not Started
last_updated: "2026-06-12"
---
# TC-INFRA-003: NetworkPolicy for pod-to-pod communication

**Objective**: Validate that NetworkPolicies correctly allow required
communication paths between OGX, vLLM, and PostgreSQL pods
while blocking unauthorized access from other pods.

**Preconditions**:
- OGX deployed on port 8321
- vLLM serving on port 8000
- PostgreSQL on port 5432
- NetworkPolicies applied for all three services
- A test pod deployed in the same namespace for negative testing

**Test Steps**:
1. Deploy NetworkPolicies restricting ingress to vLLM (port 8000)
   and PostgreSQL (port 5432) to only allow traffic from
   OGX pods.
2. From the OGX pod, verify connectivity to vLLM:
   ```bash
   oc exec deploy/llamastack -n <namespace> -- \
     curl -s -o /dev/null -w "%{http_code}" \
     http://vllm:8000/health
   ```
3. From the OGX pod, verify connectivity to PostgreSQL:
   ```bash
   oc exec deploy/llamastack -n <namespace> -- \
     pg_isready -h postgresql -p 5432
   ```
4. From a test pod (not OGX), attempt to reach vLLM directly within
   the configured timeout window:
   ```bash
   oc exec deploy/test-pod -n <namespace> -- \
     curl -s --connect-timeout <timeout> http://vllm:8000/health
   ```
5. From the test pod, attempt to reach PostgreSQL directly:
   ```bash
   oc exec deploy/test-pod -n <namespace> -- \
     pg_isready -h postgresql -p 5432 -t <timeout>
   ```

**Expected Results**:
- OGX → vLLM (port 8000): connection succeeds, HTTP 200
- OGX → PostgreSQL (port 5432): connection succeeds,
  pg_isready reports accepting connections
- test-pod → vLLM (port 8000): connection refused or timeout
  within the configured timeout window
- test-pod → PostgreSQL (port 5432): connection refused or
  timeout within the configured timeout window

**Notes**: To be filled later in the process.

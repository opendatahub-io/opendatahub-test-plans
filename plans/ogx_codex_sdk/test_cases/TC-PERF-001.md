---
test_case_id: TC-PERF-001
source_key: RHAISTRAT-1456
priority: P0
status: Draft
automation_status: Not Started
last_updated: "2026-06-12"
---
# TC-PERF-001: TTFT under 200ms at p95

**Objective**: Validate that streaming time-to-first-token (TTFT) is
under 200ms at p95 on A100-80GB or H100 GPU hardware.

**Preconditions**:
- LlamaStack deployed on port 8321
- vLLM serving target OSS model on port 8000 with A100-80GB or
  H100 GPU
- No concurrent load on the cluster during baseline measurement
- Network latency between test client and cluster < 5ms

**Test Steps**:
1. Prepare a simple prompt (no tool definitions):
   `{"model": "<target_model>", "messages": [{"role": "user",
   "content": "Explain what a Kubernetes pod is in one
   sentence."}], "stream": true}`
2. Send 100 sequential requests to
   `http://<llamastack>:8321/v1/chat/completions`
3. For each request, record:
   - `t_request`: timestamp immediately before sending the
     HTTP request
   - `t_first_chunk`: timestamp when the first SSE `data:`
     chunk (not the HTTP header) is received
   - `ttft = t_first_chunk - t_request`
4. Calculate p50, p95, and p99 from the 100 TTFT measurements
5. Repeat with tool definitions included in the request to
   measure tool-call-aware TTFT

**Expected Results**:
- p95 TTFT < 200ms for simple prompts without tools
- p50 TTFT < 100ms for simple prompts without tools
- p99 TTFT < 500ms (soft target)
- Tool-call-aware TTFT does not exceed 1.5x baseline

**Test Data**:
```bash
# Measurement script outline
for i in $(seq 1 100); do
  t_start=$(date +%s%N)
  # Send streaming request, capture first data: line
  first_chunk_time=$(curl -s -w '%{time_starttransfer}' \
    -X POST http://<llamastack>:8321/v1/chat/completions \
    -H "Content-Type: application/json" \
    -d '{"model":"<model>","stream":true,"messages":[
      {"role":"user","content":"Explain a K8s pod"}]}' \
    -o /dev/null 2>&1)
  echo "$i,$first_chunk_time"
done | tee ttft_results.csv
```

**Notes**: To be filled later in the process.

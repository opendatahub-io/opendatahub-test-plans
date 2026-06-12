---
test_case_id: TC-PERF-002
source_key: RHAISTRAT-1456
priority: P1
status: Draft
automation_status: Not Started
last_updated: "2026-06-12"
---
# TC-PERF-002: LlamaStack proxy overhead

**Objective**: Measure the latency overhead introduced by the
LlamaStack proxy layer between the client and vLLM, confirming
it stays under 5ms per hop at p95.

**Preconditions**:
- LlamaStack deployed on port 8321
- vLLM serving target model on port 8000
- Both services accessible from the test client pod
- No other load on the cluster during measurement

**Test Steps**:
1. Prepare an identical request payload:
   `{"model": "<target_model>", "messages": [{"role": "user",
   "content": "What is 2 + 2?"}], "stream": true}`
2. Send 50 requests directly to vLLM at
   `http://<vllm>:8000/v1/chat/completions` and record TTFT
   for each
3. Send 50 identical requests through LlamaStack at
   `http://<llamastack>:8321/v1/chat/completions` and record
   TTFT for each
4. Calculate the per-request overhead:
   `overhead = ttft_llamastack - ttft_vllm`
5. Compute p50, p95, and p99 of the overhead distribution

**Expected Results**:
- p95 proxy overhead < 5ms
- p50 proxy overhead < 2ms
- No requests where LlamaStack adds > 20ms overhead (outlier
  threshold)
- LlamaStack FastAPI/Uvicorn logs show no warning-level
  latency entries

**Notes**: To be filled later in the process.

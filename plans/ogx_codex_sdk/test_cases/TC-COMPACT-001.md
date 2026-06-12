---
test_case_id: TC-COMPACT-001
source_key: RHAISTRAT-1456
priority: P1
status: Draft
automation_status: Not Started
last_updated: "2026-06-12"
---
# TC-COMPACT-001: Compaction triggers at context window limit

**Objective**: Validate that the Compaction API automatically compresses
conversation history when the session approaches the model's context
window limit.

**Preconditions**:

- LlamaStack deployed on port 8321 with Compaction API provider
  registered in config.yaml
- PostgreSQL session store available on port 5432
- Target model served via vLLM (e.g., Llama 3.3 70B with 128K
  context window)
- Compaction API (upstream PR #5327) merged or cherry-picked

**Test Steps**:

1. Create a new session by sending a POST to
   `/v1/chat/completions` with a unique `session_id` and
   `stream=true`
2. Send 100+ sequential tool-call turns, each producing ~1K tokens
   of context (e.g., `read_file` on progressively larger files),
   accumulating toward the 128K context window
3. Monitor LlamaStack logs for Compaction API invocation
4. After compaction triggers, send a follow-up prompt asking the
   model to summarize the first 5 files read in the session
5. Query PostgreSQL to inspect session state metadata

**Expected Results**:

- Compaction API is invoked automatically before context window
  overflow (visible in LlamaStack FastAPI/Uvicorn logs)
- Post-compaction session state contains a compressed summary
  that is shorter than the raw conversation history
- Model correctly references key facts from pre-compaction turns
  (file names, content patterns) in its summary response
- No HTTP 500 errors or context window overflow errors during
  the session

**Validation**:

- `psql -h localhost -p 5432 -c "SELECT session_id,
  compaction_timestamp, original_token_count,
  compacted_token_count FROM session_compaction_log
  WHERE session_id = '<session_id>';"` returns a row with
  `compacted_token_count < original_token_count`

**Notes**: To be filled later in the process.

---
test_case_id: TC-SESS-004
source_key: RHAISTRAT-1456
priority: P1
status: Draft
automation_status: Not Started
last_updated: "2026-06-12"
---
# TC-SESS-004: Invalid session_id handling

**Objective**: Verify graceful handling when a non-existent
`session_id` is provided to `/v1/chat/completions`.

**Test Steps**:
1. Generate a random UUID that does not exist in the PostgreSQL
   session store
2. POST to `/v1/chat/completions` on port 8321 with the non-existent
   `session_id` and a prompt requesting `read_file` on a known file
3. Observe the HTTP response code and body
4. If the request succeeds (new session created), send a follow-up
   turn with the same `session_id` referencing prior context

**Expected Results**:
- LlamaStack does not return HTTP 500 or crash
- One of two acceptable behaviors:
  - HTTP 200 with a new session created (empty context, no stale data
    from other sessions)
  - HTTP 404 or 422 with a clear error message indicating the
    session_id was not found
- If a new session is created, the follow-up turn (step 4) correctly
  stores and retrieves the new session context
- No stale or orphaned data from other sessions is returned

**Notes**: To be filled later in the process.

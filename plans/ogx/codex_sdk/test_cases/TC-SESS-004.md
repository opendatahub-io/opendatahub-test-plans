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
4. Verify the response body contains a clear error explaining that
   the session was not found or is not owned by the authenticated
   principal

**Expected Results**:

- OGX does not return HTTP 500 or crash
- HTTP 404 or 422 with a clear error message indicating the
  `session_id` was not found or is not accessible to the caller
- No stale or orphaned data from other sessions is returned

**Notes**: To be filled later in the process.

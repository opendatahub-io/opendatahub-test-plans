---
test_case_id: TC-COMPACT-003
source_key: RHAISTRAT-1456
priority: P2
status: Draft
automation_status: Not Started
last_updated: "2026-06-12"
---
# TC-COMPACT-003: Compaction with concurrent sessions

**Objective**: Validate that compaction of one session does not
interfere with other active sessions running concurrently.

**Preconditions**:

- OGX deployed with Compaction API provider enabled
- PostgreSQL session store on port 5432
- Sufficient resources for 2+ concurrent sessions

**Test Steps**:

1. Start session A (`session_id=sess-a-<uuid>`) and session B
   (`session_id=sess-b-<uuid>`) concurrently
2. In session A, send enough turns to reach the configured
   compaction threshold
3. While session A is approaching compaction, actively use
   session B with tool-call turns (e.g., `bash`, `read_file`)
4. Observe session B behavior during and after session A's
   compaction event
5. Verify session B's conversation context is intact by
   asking it to recall facts from its own earlier turns

**Expected Results**:

- Session B continues to respond normally during session A's
  compaction (no timeouts, no stalled responses)
- Session B's conversation history is unaffected — no turns
  lost, no cross-session context contamination
- Storage coordination during compaction does not block session B's
  normal reads or writes
- Both sessions remain functional after compaction completes

**Notes**: To be filled later in the process.

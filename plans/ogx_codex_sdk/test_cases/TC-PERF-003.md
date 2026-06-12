---
test_case_id: TC-PERF-003
source_key: RHAISTRAT-1456
priority: P2
status: Draft
automation_status: Not Started
last_updated: "2026-06-12"
---
# TC-PERF-003: Concurrent session performance

**Objective**: Validate PostgreSQL session store performance under
concurrent session load, ensuring session retrieval stays under
10ms at p95.

**Preconditions**:
- LlamaStack deployed on port 8321 with PostgreSQL session store
- PostgreSQL on port 5432 with connection pooling configured
- Sufficient cluster resources for 100 concurrent sessions
- Target model served via vLLM on port 8000

**Test Steps**:
1. Start 100 concurrent Codex CLI sessions, each with a unique
   `session_id`
2. Each session performs 10 tool-call turns sequentially (mix of
   `bash`, `read_file`, and `grep_search` calls)
3. Measure per-turn latency for each session, focusing on the
   session state retrieval component (time between request
   received and first vLLM call)
4. Monitor PostgreSQL:
   - Active connections via `pg_stat_activity`
   - Connection pool utilization
   - Query execution time via `pg_stat_statements`
5. After all sessions complete, verify no sessions were dropped
   or returned stale state

**Expected Results**:
- Session state retrieval latency < 10ms at p95 across all
  100 sessions
- No PostgreSQL connection pool exhaustion (active connections
  remain below pool max)
- All 100 sessions complete their 10 turns without error
- No dropped sessions or HTTP 503 responses

**Validation**:
- `psql -h localhost -p 5432 -c "SELECT count(*) FROM
  pg_stat_activity WHERE datname = 'llamastack';"` stays
  below connection pool limit throughout the test
- `psql -h localhost -p 5432 -c "SELECT query, mean_exec_time
  FROM pg_stat_statements WHERE query LIKE '%session%'
  ORDER BY mean_exec_time DESC LIMIT 5;"` shows mean
  execution time < 10ms for session queries

**Notes**: To be filled later in the process.

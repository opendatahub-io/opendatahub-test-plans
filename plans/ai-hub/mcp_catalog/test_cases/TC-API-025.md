# TC-API-025: Transport Derivation for Remote Servers

**Priority**: P1
**Objective**: Verify transports are derived from endpoints for remote servers

**Preconditions**:

- Remote server with HTTP endpoint defined

**Test Steps**:

1. Load remote server with http endpoint
2. Get server via API
3. Verify transports array includes "http"
4. Load remote server with sse endpoint
5. Get server via API
6. Verify transports array includes "sse"
7. Load server with both endpoints
8. Verify transports array includes both

**Expected Results**:

- HTTP endpoint → transports includes "http"
- SSE endpoint → transports includes "sse"
- Both endpoints → transports includes both
- Transport derivation automatic for remote servers

**Test Data**:

```yaml
# Server with HTTP endpoint
name: http-server
deploymentMode: remote
endpoints:
  http: https://api.example.com/mcp

# Server with SSE endpoint
name: sse-server
deploymentMode: remote
endpoints:
  sse: https://stream.example.com/mcp
```

**Expected Response**:

```json
{
  "name": "http-server",
  "transports": ["http"],
  "endpoints": {
    "http": "https://api.example.com/mcp"
  }
}
```

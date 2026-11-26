# Contracts v0.1

## Limits (initial defaults)
- Job timeout: 30s
- Max response body: 512 KB
- Max job payload: 16 KB
- One job in flight; queue in Node only

## Job (Node -> extension)
Fields: jobId, opId, params (schema-validated), deadline, attempt
Forbidden fields: url, script, headers, cookies, tokens

## Result (extension -> Node)
Fields: jobId, ok, status, contentType, body (capped), errorCode
Forbidden data: cookies, Set-Cookie, auth/CSRF headers, page HTML

## Error codes
EXECUTOR_OFFLINE, TAB_NOT_FOUND, MULTIPLE_TABS, LOGIN_REQUIRED,
CSRF_FAILED, CORS_BLOCKED, RATE_LIMITED, TIMEOUT,
RESPONSE_TOO_LARGE, UNKNOWN_OPERATION, INVALID_PARAMS, UPSTREAM_ERROR

## Job lifecycle
queued -> claimed (leased) -> succeeded / failed / expired
- Lease expires if the tab closes or the worker disconnects.
- Never auto-retry non-idempotent operations.
- Duplicate jobId must never execute twice.

## Message types
ping/pong, job_available, job_result, executor_status, shutdown
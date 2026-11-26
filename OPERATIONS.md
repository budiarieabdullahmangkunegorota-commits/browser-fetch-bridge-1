# Operation Catalog

Only operations in this file may run. Adding one requires a Phase 7
review entry in update.md.

## OP-001 — placeholder, first read-only operation
- Method/path: GET /FIXED/PATH (same target origin)
- Params: none, or list each allowed param + type + max length
- Auth context: page-context fetch, normal browser cookies
- Timeout / max body: 30s / 512 KB
- Retry: none
- Response: JSON only; fields Node is allowed to receive: list them
- Errors mapped to: LOGIN_REQUIRED, CORS_BLOCKED, UPSTREAM_ERROR

## Template for new operations
- ID, method, fixed path, allowed params/body schema
- Idempotent? yes/no. CSRF requirement if a write — stop and review
  SECURITY.md before adding any POST/PUT/DELETE operation.
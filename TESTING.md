# Test Plan

## Rules
- Test against a mock first, then the real target (Phase 5 onward).
- Record every run in update.md: phase, test IDs, result, evidence.
- A phase cannot be marked Passed with any failing or skipped test.

## Categories to cover by Phase 8
Connectivity, tab states, login expiry, CORS/third-party-cookie
failure, CSRF (writes only), timeout/abort, oversized responses,
duplicate-job prevention, reconnect after host/extension/Chrome
restart, redacted logs, clean install/uninstall.

## Always verify
- Node payloads and logs contain no target cookies or tokens.
- Executor refuses unknown operations and forbidden job fields.
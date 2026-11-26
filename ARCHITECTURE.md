# Architecture

## Flow
Node app
  -> job queue (named operation only)
  -> Native Messaging host (stdio, framed JSON)
  -> Chrome extension (MV3 service worker)
  -> pinned target tab (page-context fetch)
  -> target API, using the browser's normal cookies
  -> sanitized result returns on the same path

## Components

### Node app
- Owns job queue, job IDs, leases, timeouts, result validation.
- Validates operation ID and parameters against OPERATIONS.md.
- Never receives target cookies/tokens and never calls the target API.

### Native Messaging host
- Thin shim launched by Chrome. Forwards jobs/results between the
  extension and the Node app over user-only local IPC.
- stdout is protocol only. Logs go to stderr/files.
- Host manifest allows only the exact extension ID.

### Chrome extension
- Minimal permissions: nativeMessaging, scripting, storage, exact
  target host permission. No `cookies` permission.
- Maintains native connection, reconnects after service-worker suspension.
- Finds exactly one pinned target tab, validates origin.
- Executes only catalogued operations in the correct page context.
- Returns only status, content-type, and a size-limited body.

### Target tab
- Must be open, pinned, on TARGET_PAGE_URL, and logged in.
- Page-context execution is observable by the target page; never
  pass Node-side secrets into page context.

## Non-goals
- Transparent proxy, arbitrary URL fetcher, cookie export,
  login/MFA/CAPTCHA automation, multi-user service.

## Fallback (Phase 9 only)
Playwright/CDP with a dedicated, manually logged-in browser profile.
Separate cookie jar from normal Chrome. Never attach to the user's
default Chrome profile and never expose a debug port to the network.
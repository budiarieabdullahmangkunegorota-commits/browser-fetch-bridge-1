# Browser Fetch Bridge

Local, single-user bridge where normal Chrome performs authenticated
`fetch` calls with its own session, and a local Node app only queues
named operations and receives sanitized results.

Chrome Extension + Native Messaging is the primary executor.
Playwright/CDP with a dedicated profile is only a fallback decision,
not built unless Phase 9 gate is approved.

## Non-negotiables
- Browser owns target-site cookies/auth. Node never receives them.
- No arbitrary URL, arbitrary JavaScript, or arbitrary headers from Node.
- Only operations listed in OPERATIONS.md may run.
- No cookie, access token, refresh token, CSRF token, or `Set-Cookie`
  value may be sent to Node, logged, or stored outside the browser.

## Placeholders to fill in Phase 0
- TARGET_ORIGIN: https://TARGET.example
- TARGET_PAGE_URL: https://TARGET.example/PAGE
- FIRST_OPERATION: OP-001
- OS: Windows / macOS / Linux
- Chrome version:

## Phase status
| Phase | Name | Status |
|---|---|---|
| 0 | Preflight and safety | Not started |
| 1 | Contracts and operation catalog | Not started |
| 2 | Node job queue + mock executor | Not started |
| 3 | Native Messaging skeleton | Not started |
| 4 | Target tab detection/pinning | Not started |
| 5 | First read-only fetch end-to-end | Not started |
| 6 | Hardening and MV3 lifecycle | Not started |
| 7 | Expand operations | Not started |
| 8 | Install, packaging, uninstall | Not started |
| 9 | Optional Playwright fallback gate | Not started |

Status values: Not started / In progress / Passed / Failed / Blocked
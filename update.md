# Update Log

Append-only. Never edit past entries; add a correction entry instead.
One entry per work or test session.

## Entry template
### YYYY-MM-DD HH:MM — Phase N — Session title
- Status: In progress / Passed / Failed / Blocked
- Done:
- Tests run: P#-T# pass/fail + evidence
- Issues found:
- Decisions:
- Next step:

## Log
### [DATE] — Project setup
- Status: In progress
- Done: Documentation pack created (README, ARCHITECTURE, SECURITY,
  CONTRACTS, OPERATIONS, PHASES, TESTING, update.md).
- Tests run: none yet.
- Issues found: session values were previously exposed in chat —
  revocation/rotation required in Phase 0.
- Decisions: Extension + Native Messaging is primary; Playwright/CDP
  is a Phase 9 fallback gate only. No arbitrary URLs or cookie export.
- Next step: complete Phase 0 checklist.
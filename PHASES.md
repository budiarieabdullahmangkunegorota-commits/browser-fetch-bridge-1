# Phases — build and test one at a time

Rule: a phase is Passed only when every test below is checked,
evidence is recorded in update.md, and README status is updated.

## Phase 0 — Preflight and safety
Build: fill README placeholders; revoke any exposed sessions;
confirm the target site's terms allow this use; record OS/Chrome.
Tests:
- [ ] P0-T1 Logged into target in normal Chrome; target page loads.
- [ ] P0-T2 No secrets/cookies/tokens exist in project files.
- [ ] P0-T3 One read-only endpoint identified for OP-001.
Pass: go/no-go recorded in update.md. Stop if terms disallow it.

## Phase 1 — Contracts and operation catalog
Build: freeze CONTRACTS.md v0.1 and OP-001 in OPERATIONS.md.
Tests:
- [ ] P1-T1 Spec contains no arbitrary URL/script/header field.
- [ ] P1-T2 Every error code and limit has an owner (Node or extension).
Pass: contracts reviewed and marked frozen in update.md.

## Phase 2 — Node job queue + mock executor
Build: Node queue, leases, timeouts, result validation, redacted logs.
No browser, no target calls. Mock executor returns canned results.
Tests:
- [ ] P2-T1 Enqueue -> claim -> succeed returns the canned result.
- [ ] P2-T2 Lease expiry, timeout, unknown op, oversized result fail
      with the correct CONTRACTS error codes.
- [ ] P2-T3 Logs contain no cookies/tokens (inspect manually).
Pass: all mock tests green without Chrome involved.

## Phase 3 — Native Messaging skeleton
Build: MV3 extension skeleton + native host shim; ping/pong only.
Tests:
- [ ] P3-T1 Extension connects; ping/pong round-trips.
- [ ] P3-T2 Host crash, extension reload, and Chrome restart reconnect.
- [ ] P3-T3 Malformed message rejected; stdout carries protocol only.
- [ ] P3-T4 Service-worker suspension does not lose queued jobs.
Pass: stable reconnect demonstrated three times.

## Phase 4 — Target tab detection and pinning
Build: tab discovery by exact origin, single-tab enforcement, status UI
(connected / no tab / multiple tabs / wrong page). No fetch yet.
Tests:
- [ ] P4-T1 0 tabs, 1 tab, 2 tabs, navigated-away tab each report the
      correct status/error.
- [ ] P4-T2 Login page detected as LOGIN_REQUIRED, never auto-logged-in.
Pass: tab state is always reported correctly.

## Phase 5 — First read-only fetch end-to-end
Build: OP-001 executor in the correct page context; sanitized result
to Node per CONTRACTS.md.
Tests:
- [ ] P5-T1 Logged-in tab returns authenticated OP-001 JSON to Node.
- [ ] P5-T2 Logged-out returns LOGIN_REQUIRED; Node gets no data.
- [ ] P5-T3 Timeout and >512 KB body return TIMEOUT /
      RESPONSE_TOO_LARGE.
- [ ] P5-T4 Concurrent jobs are serialized; no duplicates.
- [ ] P5-T5 Inspect payloads/logs: no Cookie/Set-Cookie/token present.
Pass: P5-T1..T5 all green on the real target.

## Phase 6 — Hardening and MV3 lifecycle
Build: backpressure, rate limit, lease recovery, clear status/errors.
Tests:
- [ ] P6-T1 Kill host / close tab / reload extension mid-job: job ends
      as failed or expired, never hangs.
- [ ] P6-T2 Chrome restart resumes cleanly with no stuck queue.
Pass: no hung jobs across all fault tests.

## Phase 7 — Expand operations
Build: add one operation at a time, same template as OP-001.
Tests: repeat the Phase 5 test set for each new operation; full
regression of earlier operations after each addition.
Pass: each operation individually signed off in update.md.

## Phase 8 — Install, packaging, uninstall
Build: stable extension ID, per-OS host registration steps,
install/uninstall instructions, permissions review.
Tests:
- [ ] P8-T1 Clean install from docs only reaches a passing OP-001.
- [ ] P8-T2 Uninstall removes host registration and local data.
Pass: another clean run reproduces P8-T1.

## Phase 9 — Optional Playwright fallback gate (decision only)
Decide only if execution must run without a normal-Chrome tab.
If approved: dedicated profile, manual login, Playwright or CDP
(choose one), same OPERATIONS.md and CONTRACTS.md apply.
Tests if built: profile login persistence, session-expiry detection,
and the full Phase 5/6 test sets against the dedicated profile.
# Security Rules

## Red lines — never do
- Never read `document.cookie` or use the `cookies` permission.
- Never send `Cookie`, `Set-Cookie`, Authorization, CSRF, access, or
  refresh-token values to Node.
- Never accept a URL, script, method override, or header set from Node.
- Never disable Chrome web security or log into the target from Node.
- Never expose Native Messaging, Node, or a debug port on 0.0.0.0/LAN.

## Rules
- Operation catalog is duplicated: Node validates, extension re-validates.
- Response body cap and timeout enforced in extension and Node.
- Redact cookies, tokens, and sensitive bodies from all logs.
- Rate-limit and serialize jobs; one executor tab only.
- Keep extension ID stable; host manifest allows only that ID.
- Treat all target page content and API bodies as untrusted input.

## Incident note
Any session/token pasted into chat, logs, tickets, or files is treated
as exposed: log out that session and rotate refresh tokens before Phase 1.
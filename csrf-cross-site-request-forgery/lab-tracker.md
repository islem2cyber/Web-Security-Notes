# CSRF Lab Tracker

This tracker records the verified PortSwigger Web Security Academy CSRF progress provided for this learning path.

## Labs

| # | Lab name | Difficulty | Status | Lab documentation |
|---:|---|---|---|---|
| 1 | CSRF vulnerability with no defenses | Apprentice | SOLVED | [View notes](./labs/csrf-vulnerability-with-no-defenses.md) |
| 2 | CSRF where token validation depends on request method | Practitioner | SOLVED | [View notes](./labs/token-validation-depends-on-request-method.md) |
| 3 | CSRF where token validation depends on token being present | Practitioner | SOLVED | [View notes](./labs/token-validation-depends-on-token-being-present.md) |
| 4 | CSRF where token is not tied to user session | Practitioner | SOLVED | [View notes](./labs/token-not-tied-to-user-session.md) |
| 5 | CSRF where token is tied to non-session cookie | Practitioner | SOLVED | [View notes](./labs/token-tied-to-non-session-cookie.md) |
| 6 | CSRF where token is duplicated in cookie | Practitioner | SOLVED | [View notes](./labs/token-duplicated-in-cookie.md) |
| 7 | SameSite Lax bypass via method override | Practitioner | SOLVED | [View notes](./labs/samesite-lax-bypass-via-method-override.md) |
| 8 | SameSite Strict bypass via client-side redirect | Practitioner | SOLVED | [View notes](./labs/samesite-strict-bypass-via-client-side-redirect.md) |
| 9 | SameSite Strict bypass via sibling domain | Practitioner | POSTPONED — NOT SOLVED | [View notes](./labs/samesite-strict-bypass-via-sibling-domain.md) |
| 10 | CSRF where Referer validation depends on header being present | Practitioner | SOLVED | [View notes](./labs/referer-validation-depends-on-header-being-present.md) |
| 11 | CSRF with broken Referer validation | Practitioner | SOLVED | [View notes](./labs/csrf-with-broken-referer-validation.md) |

## Progress summary

| Difficulty | Solved | Postponed | Total |
|---|---:|---:|---:|
| Apprentice | 1 | 0 | 1 |
| Practitioner | 9 | 1 | 10 |
| **Total** | **10** | **1** | **11** |

- Solved: 10
- Postponed: 1
- Apprentice: 1/1 solved
- Practitioner: 9/10 solved
- Overall completion: 10/11

## Postponed lab

The sibling-domain lab is intentionally postponed. It involves Cross-Site WebSocket Hijacking (CSWSH), and the official lab description recommends learning WebSocket vulnerabilities first.

- Status: POSTPONED
- Reason: Requires prior understanding of WebSockets and Cross-Site WebSocket Hijacking.
- Planned revisit: Advanced Web Security - WebSockets Security, later in Phase 4 of the cybersecurity roadmap.

This is not a failed attempt and must not be presented as solved.

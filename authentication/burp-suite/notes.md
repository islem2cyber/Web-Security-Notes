# Burp Suite for Authentication Study

This is a learning reference for using Burp Suite to understand authentication flows in authorized PortSwigger labs or other explicitly scoped environments. It is not a claim of mastery. Set scope before testing, use test accounts, and avoid requests that could lock out users or disrupt a service.

## Useful tools

| Tool | Authentication-study use |
|---|---|
| Proxy | Observe browser requests, responses, redirects, and cookies while following a flow. |
| HTTP history | Reconstruct endpoint order and compare how session state changes across a workflow. |
| Repeater | Make small, deliberate edits to a captured request and compare controlled responses. |
| Intruder | Perform bounded, authorized request variation where a lab requires it; use conservative limits and do not target real accounts. |
| Decoder | Inspect or transform encoded values for analysis; encoding is not encryption. |
| Comparer | Compare captured requests or responses, accounting for dynamic values. |
| Sequencer | Assess randomness of generated tokens in an authorized test context; statistical output is evidence, not proof by itself. |
| Logger | Review tool-generated activity and troubleshoot request/session behavior. |

Tool availability and names can vary between Burp editions and versions.

## Scope and authenticated sessions

Define only the approved host and paths as in scope, and confirm the active target before sending requests. Use a dedicated test account. Identify the cookies and state transitions used by the application, redact secrets from notes, and verify that the session remains valid and belongs to the expected test identity.

When a workflow includes per-request CSRF tokens, rotating session cookies, or MFA challenges, a copied request may become stale. Understand the sequence in Proxy history before automating it. Avoid sharing or exporting live session tokens.

For stateful automation concepts, see [Macros and session handling](./macros-and-session-handling.md). For Intruder terminology and attack-type differences, see [Intruder](./intruder.md).

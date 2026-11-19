# CSRF where token is tied to non-session cookie

- **Difficulty:** Practitioner
- **Status:** SOLVED

## Vulnerability concept

The CSRF token is associated with a separate cookie rather than the authenticated server-side session.

## Root cause

The application treats a non-session cookie or user-controlled cookie state as the trust anchor for CSRF validation.

## Technical explanation

If the cookie is predictable, settable by another context, or not bound to the authenticated identity, the server can accept a token relationship without verifying that it belongs to the current session. Separate session and anti-CSRF state must remain verifiable by the server.

Exact lab payloads are intentionally omitted.

## What the lab teaches

- Cookie presence is not sufficient session binding.
- Domain, path, and cookie-setting ability affect trust.
- The application must bind request state to authenticated session state rather than an auxiliary token alone.

## Security implications

An attacker may establish or influence the required cookie state and supply a matching request value.

## Proper remediation

- Generate server-side anti-CSRF state bound to the authenticated session.
- Do not depend on a cookie the application cannot securely control.
- Apply Domain and Path attributes narrowly and use Secure and appropriate SameSite settings.
- Use framework-provided protections and integrity where available.

## Key takeaways

The critical relation is not "cookie plus parameter" but an unpredictable token bound to trusted server-side session state.

## Personal notes

To be added.

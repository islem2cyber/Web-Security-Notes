# CSRF where token validation depends on token being present

- **Difficulty:** Practitioner
- **Status:** SOLVED

## Vulnerability concept

A server validates a CSRF token when supplied but accepts a state-changing request without one.

## Root cause

Validation is conditional on the token parameter existing rather than mandatory for every protected operation.

## Technical explanation

A missing token should be a validation failure. Falling through to normal processing creates an unauthenticated bypass and defeats the purpose of anti-CSRF state.

No exact lab request or payload is reproduced here.

## What the lab teaches

- Missing and invalid tokens must follow the same secure failure path.
- Optional validation is not an access-control requirement.
- Middleware or endpoint-level guards should run before the mutation.

## Security implications

The token can be entirely omitted from a request, allowing browser and application behavior to determine whether the session cookie is sufficient.

## Proper remediation

- Require a token on every CSRF-sensitive mutation.
- Reject missing, stale, malformed, or mismatched values.
- Use uniform error handling that does not reveal session or token state unnecessarily.

## Key takeaways

A defense that validates only if a value is present is not a defense at all.

## Personal notes

To be added.

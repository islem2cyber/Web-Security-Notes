# CSRF where Referer validation depends on header being present

- **Difficulty:** Practitioner
- **Status:** SOLVED

## Vulnerability concept

The application rejects mismatched Referer headers but accepts requests when the header is absent.

## Root cause

Referer presence is incorrectly used as a conditional requirement rather than as a strict, fail-closed trust decision.

## Technical explanation

Browsers and privacy mechanisms may suppress the Referer. If application logic allows missing metadata, an attacker can use a context or browser configuration that omits the header.

This general explanation does not provide exact lab payloads or observations.

## What the lab teaches

- Header absence has legitimate reasons and is not proof of the request origin.
- A conditional `if present, validate` policy creates an explicit bypass.
- Referer validation is optional browser metadata, not a primary CSRF primitive.

## Security implications

Mutations may proceed without either a valid CSRF token or a confirmable referrer origin.

## Proper remediation

- Implement synchronized, session-bound CSRF tokens.
- Where Referer validation is also used, define an explicit and documented fail-open/fail-closed policy appropriate to compatibility requirements.
- Do not treat missing metadata as same-origin evidence.
- Use Origin validation where supported.

## Key takeaways

Referer validation must complement a stronger control and must not turn header absence into approval.

## Personal notes

To be added.

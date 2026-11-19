# CSRF where token is not tied to user session

- **Difficulty:** Practitioner
- **Status:** SOLVED

## Vulnerability concept

The application accepts a validly formatted CSRF token issued to a different authenticated session.

## Root cause

The token is unique to the application or user-population but is not cryptographically and logically bound to the victim's server-side session.

## Technical explanation

An attacker may obtain a token in their own session and submit it where the victim session performs a mutation. If the server validates only token validity rather than session ownership, the token loses its anti-CSRF property.

This explanation is general; exact lab-specific evidence is not supplied.

## What the lab teaches

- Randomness alone does not establish session intent.
- Tokens must be associated with trusted server-side session state.
- Cross-session token acceptance is a root-cause implementation defect.

## Security implications

An attacker can reuse a controlled token against a victim authenticated session.

## Proper remediation

- Store token-to-session binding server-side.
- Validate token and authenticated session together.
- Rotate token and session state after login or privilege changes.
- Prevent an attacker-controlled session from minting valid values for another session.

## Key takeaways

A CSRF token proves only what the server binds it to. Session binding is essential.

## Personal notes

To be added.

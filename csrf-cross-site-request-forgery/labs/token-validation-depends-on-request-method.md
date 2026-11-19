# CSRF where token validation depends on request method

- **Difficulty:** Practitioner
- **Status:** SOLVED

## Vulnerability concept

The application validates a CSRF token for some methods but not for the logical state-changing operation reached by another method.

## Root cause

Anti-CSRF enforcement is attached to HTTP method dispatch instead of to the server-side mutation itself.

## Technical explanation

Equivalent operations can sometimes be reached with alternative methods or method overrides. If only `POST` receives validation while `GET`, `PUT`, or an override reaches the mutation, the control is bypassed by request representation.

This is a general explanation, not a claim of the exact lab payload or request sequence.

## What the lab teaches

- Token enforcement must cover the operation, not one nominal method.
- State changes must be protected consistently across routes and methods.
- Testing should include supported alternatives and server method handling.

## Security implications

An attacker may trigger the state change with an accepted unprotected representation.

## Proper remediation

- Reject unsupported state-changing methods.
- Apply CSRF validation consistently to every accepted mutation representation.
- Enforce framework middleware on routed operations before they execute.
- Avoid method overrides unless explicitly required and protected.

## Key takeaways

Security is determined by the operation and server-side state transition, not by a method label alone.

## Personal notes

To be added.

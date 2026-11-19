# SameSite Lax bypass via method override

- **Difficulty:** Practitioner
- **Status:** SOLVED

## Vulnerability concept

SameSite=Lax behavior can be bypassed when the browser sends a request in a context that carries the session cookie and server middleware interprets a method override as a state-changing method.

## Root cause

The application permits method overriding and enforces CSRF assumptions at the browser-visible method or framework route rather than on the final logical operation.

## Technical explanation

Browsers may attach Lax cookies to certain top-level navigations. If a navigation ultimately reaches server logic through a method override, state-changing behavior can occur in a context that an application intended to protect with SameSite.

This is general browser and server behavior, not a verified lab payload or exact solution sequence.

## What the lab teaches

- SameSite is a cookie delivery policy, not application authorization.
- Server middleware can change the effective method.
- Lax has context-dependent exceptions and browser behavior matters.

## Security implications

An operation expected to require strict cross-site protection may execute because the browser sends cookies under an allowed navigation context.

## Proper remediation

- Reject state changes through unsafe top-level GET-style flows.
- Disallow unnecessary method overrides.
- Enforce CSRF tokens for every state-changing operation.
- Treat SameSite as defense-in-depth.

## Key takeaways

Cookie mode and HTTP method must both be mapped to the server's final operation.

## Personal notes

To be added.

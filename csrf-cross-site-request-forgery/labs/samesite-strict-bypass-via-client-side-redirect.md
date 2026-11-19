# SameSite Strict bypass via client-side redirect

- **Difficulty:** Practitioner
- **Status:** SOLVED

## Vulnerability concept

An application or redirect flow can create a same-site browser context that causes Strict cookies to be sent to a protected endpoint.

## Root cause

The application trusts a client-side navigation or redirect boundary to preserve a security property that browsers may interpret differently from server-side intent.

## Technical explanation

Strict cookies are withheld in cross-site contexts, but same-site redirect chains or client-side transitions can change the effective context. The protected endpoint still needs a direct intent check rather than assuming Strict cookies make every request safe.

No exact lab exploit or payload is supplied.

## What the lab teaches

- SameSite applies to browser cookie delivery in a specific navigation context.
- Redirect behavior must be included in browser threat modeling.
- A same-site or same-origin redirect is not proof of explicit user intent.

## Security implications

A browser navigation designed by the application or an attacker could cause the browser to reach a state-changing endpoint with authentication cookies.

## Proper remediation

- Require server-side anti-CSRF tokens.
- Avoid open redirects and validate redirect targets.
- Apply Origin validation where supported.
- Keep sensitive mutations behind server-side CSRF validation; require confirmation or reauthentication separately when the action's risk warrants it.

## Key takeaways

Redirect and navigation boundaries do not replace CSRF validation.

## Personal notes

To be added.

# CSRF where token is duplicated in cookie

- **Difficulty:** Practitioner
- **Status:** SOLVED

## Vulnerability concept

The application places the token in both a cookie and a request parameter but does not provide sufficient binding or integrity.

## Root cause

The Double-Submit Cookie pattern is implemented in a way that trusts attacker-influenceable cookie state or omits session binding.

## Technical explanation

Matching two copies of a value demonstrates consistency, but it does not inherently prove that the value belongs to the victim session. Subdomain cookie setting, weak token generation, or missing server-side binding can undermine this pattern.

No exact lab exploit steps or payloads are claimed here.

## What the lab teaches

- Naive double-submit depends on stronger cookie-integrity assumptions than a synchronizer token; signed and session-bound designs can address these risks.
- Cookie scope and control are security-critical.
- A matching value must be tied to session state or protected by a stronger mechanism.

## Security implications

An attacker may be able to influence or reuse the needed cookie/request pair, depending on browser and application trust boundaries.

## Proper remediation

- Prefer a framework synchronizer-token pattern.
- Bind tokens to server-side session state.
- Protect cookie setting and Domain scope.
- Add Origin validation and appropriate SameSite/Secure settings.

## Key takeaways

Matching client-supplied values is not equivalent to server-side validation against trusted session state.

## Personal notes

To be added.

# CSRF Tokens

A CSRF token is an unpredictable server-validated value intended to prove that a state-changing request originated from the application's legitimate interaction flow.

## Synchronizer Token Pattern

The server stores a random token associated with the authenticated user session or a protected workflow. The page includes the token in a form or request, and the server validates it against trusted server-side session state before processing the mutation.

An effective flow requires:

- Cryptographically secure randomness.
- Sufficient entropy and unpredictability.
- Server-side validation against the correct session.
- Consistent validation across login, logout, profile, account, and recovery operations.
- Safe handling of failures.

## Session binding

A token must not be merely valid for "a user" or for the entire application. It should be tied to the authenticated session and, where appropriate, to a workflow or operation. Session binding prevents an attacker with a separate session from obtaining a valid token and reusing it against the victim.

Regenerate or invalidate tokens where session security requires it, such as after authentication or privilege changes.

## Token validation mistakes

Common defects include:

- Accepting a request when no token is present.
- Validating the token only for particular HTTP methods.
- Accepting a token issued to another user or session.
- Tying a token to a user-controlled cookie rather than trusted session state.
- Using the same token as a cookie and request parameter without integrity or binding.
- Treating a predictable or low-entropy value as secret.
- Using a token as the only control while failing to validate Origin or supporting browser behavior.

## Double-Submit Cookie Pattern

In a double-submit implementation, the application emits a random cookie and requires the request to include the same value through a form or header. The server compares the submitted value with the cookie.

This pattern can fail if a subdomain or attacker-controlled content can set or manipulate the relevant cookie. A modern defense should use framework-provided, cryptographically bound anti-CSRF mechanisms or a strong synchronizer-token design. Where double-submit cookies are used, additional integrity and session-binding controls are required.

## Duplicated cookie and request values

If an application duplicates a token in a cookie and a request parameter but does not bind the value to the authenticated session, an attacker may be able to influence or obtain a valid relationship. SameSite can affect cookie attachment, but it is not a substitute for server-side token validation.

## Practical remediation

- Use a maintained framework CSRF mechanism when available.
- Reject missing, malformed, stale, or mismatched tokens.
- Bind tokens to the user's server-side session.
- Protect JSON and form-based state changes consistently.
- Rotate session and CSRF state after authentication or privilege escalation.
- Add Origin validation for supported request contexts and consider Referer validation only as a secondary compatibility control.

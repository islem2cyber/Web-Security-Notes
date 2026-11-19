# CSRF Tokens

A CSRF token is an unpredictable server-validated value that provides evidence a state-changing request passed through the application's legitimate interaction flow. It does not by itself prove that a human intended the action.

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

In a double-submit implementation, the application emits a token in a cookie and requires the request to include the same value through a form or header. The server compares the submitted value with the cookie. The security depends on how the token is generated, what the server validates, and whether an attacker can set or influence the cookie.

### Comparing token strategies

| Strategy | Server validation | Main trust assumption and security property |
|---|---|---|
| Naive Double-Submit Cookie | Compare the cookie value with the value submitted in the request. | Relies on an attacker being unable to set or influence the cookie and on a sufficiently unpredictable token. The matching values are not inherently bound to the authenticated session, so cookie injection or cross-session reuse can defeat it. |
| Signed Double-Submit Cookie | Validate a cryptographic signature over a nonce and session-specific data, in addition to checking the submitted value. | Relies on a protected server-side signing key, correct signature validation, and binding to the current session. Properly implemented, this prevents a party that can merely set a cookie from forging a valid session-bound token without requiring per-token server storage. |
| Session-bound CSRF Token (Synchronizer Token) | Compare the submitted token with a value stored in or securely associated with the authenticated server-side session. | Relies on unpredictable tokens, protected session state, and consistent validation. It provides explicit server-side session binding, at the cost of storing or managing token state. |

Double-submit cookies are not inherently insecure. A naive implementation has weaker trust assumptions; signed double-submit can provide strong session binding when correctly designed, and synchronizer tokens provide a direct server-side binding. Choose and implement the approach according to the application's architecture and threat model.

## Duplicated cookie and request values

If an application duplicates a token in a cookie and a request parameter but does not protect cookie integrity or bind the value to the authenticated session, an attacker may be able to influence or obtain a matching pair. SameSite can affect cookie attachment, but it is not a substitute for token integrity and validation.

## Practical remediation

- Use a maintained framework CSRF mechanism when available.
- Reject missing, malformed, stale, or mismatched tokens.
- Bind tokens to the user's server-side session.
- Protect JSON and form-based state changes consistently.
- Rotate session and CSRF state after authentication or privilege escalation.
- Add Origin validation for supported request contexts and consider Referer validation only as a secondary compatibility control.

# Session Management and Persistent Sign-In

After authentication, a web application commonly uses a session identifier to associate requests with server-side identity and state. A session is not the same as the password, and possession of a valid session token may be equivalent to being authenticated until it expires or is revoked.

## Lifecycle and defenses

Generate session identifiers with a cryptographically secure random source and sufficient entropy. Rotate the identifier when authentication or privilege state changes to reduce session-fixation risk. Expire sessions based on an appropriate idle and absolute lifetime, invalidate them on logout, and consider revoking sessions after password changes, account recovery, or suspected compromise.

Server-side authorization must be checked for every protected operation. Do not rely on a client-side session flag as proof that a user completed MFA or is authorized for an action.

## Cookie attributes

| Attribute | Purpose and limitation |
|---|---|
| `HttpOnly` | Prevents ordinary page JavaScript from reading the cookie. It does not stop the browser from sending it or prevent all consequences of script injection. |
| `Secure` | Restricts cookie transmission to secure connections. It is not an access-control mechanism. |
| `SameSite` | Controls some cross-site cookie-sending behavior and can reduce CSRF exposure; it does not replace CSRF defenses or fix XSS. |
| Cookie expiration | A browser-side persistence directive. It does not guarantee that the server will continue accepting the token. |

A **session cookie** commonly has no explicit persistent expiration and is intended to be removed when the browser session ends, though browser behavior varies. A **persistent cookie** has an expiration or maximum age and may survive browser restarts. In either case, server-side expiration and revocation policy remain important.

## Remember-me functionality

Persistent authentication cookies are not necessarily password hashes. Implementations vary; a cookie may contain a random bearer token, a signed value, or another construction. Do not assume its format without evidence. A predictable or weakly constructed token can let an unauthorized party impersonate a user for as long as it remains valid.

Prefer a high-entropy opaque token, store a verifier or token record server-side, support rotation and revocation, scope it narrowly, and protect it with appropriate cookie attributes. Avoid embedding reusable credentials or sensitive data in a client-readable token. Provide clear controls to revoke remembered devices.

## Completed lab

**Brute-forcing a stay-logged-in cookie** is completed. The learning concept is that weakly constructed persistent authentication tokens can undermine otherwise strong login controls. It does not imply that every remember-me cookie is a password hash. See the [lab tracker](../lab-tracker.md).

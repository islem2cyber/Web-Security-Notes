# SameSite Cookies

The SameSite attribute controls whether a browser includes a cookie based on the request's site context. It reduces some CSRF exposure but does not replace CSRF protections or define authorization. Browser implementations and navigation details can affect observed behavior.

## Cookie modes

| Mode | Typical behavior |
|---|---|
| Strict | Sends the cookie with same-site requests and withholds it on cross-site requests, including cross-site top-level navigations. A later request from the destination site may be same-site and include it. |
| Lax | Generally sends the cookie with same-site requests and cross-site top-level navigations using safe methods, while withholding it in many cross-site subresource and unsafe-method contexts. |
| None | Requires `Secure` in modern browsers and allows cross-site use subject to browser third-party-cookie policies. |

Actual behavior depends on browser versions and implementation details.

## Secure and None

The `Secure` attribute requires HTTPS and prevents normal browser transmission over plaintext HTTP. `SameSite=None` indicates that cross-site usage is allowed; it does not mean HTTP is secure, and modern browsers require `Secure` for the None mode.

## Same-origin, same-site, and cross-site

An origin is a scheme, hostname, and port tuple. SameSite uses a site concept, generally based on the scheme and registrable domain (the schemeful-site model), not the origin. Ports do not distinguish sites in the way they distinguish origins.

For example, sibling subdomains using the same scheme can be same-site while having different origins. A same-site request can therefore still be cross-origin; SameSite does not make sibling origins equivalent or relax the Same-Origin Policy.

Do not treat sibling-domain requests as automatically cross-site just because they are cross-origin, or assume same-site means same-origin. Assess the scheme, registrable domain, cookie attributes, and browser request context.

## Bypasses and edge cases

### Method override

Some applications accept method override parameters or headers such as `_method`, `X-HTTP-Method-Override`, or framework-specific equivalents. A cross-site request may reach an endpoint under one browser-visible method but be interpreted as another method by server middleware.

Enforce state-changing semantics on the server and treat unexpected method overrides consistently.

### Client-side redirects

A same-site redirect or client-side navigation can change the context in which a browser sends an authenticated cookie. An application must not assume that a redirect alone provides a security boundary.

### Sibling domains

Sibling subdomains can create same-site relationships while retaining separate origins. Additional browser or application vulnerabilities may permit one sibling to affect another. A CSRF assessment must account for origin, site, cookie Domain and Path attributes, and browser-specific websocket or cross-origin behavior.

The sibling-domain lab described in this repository specifically introduces Cross-Site WebSocket Hijacking concepts and is postponed until WebSockets study.

## Limitations

SameSite is valuable defense-in-depth, but browser versions, embedded contexts, redirects, and application misconfiguration can affect observed behavior. Some browsers also apply compatibility behavior to cookies without an explicit SameSite attribute. Sensitive mutations should still use framework CSRF defenses and, for high-risk actions, explicit confirmation or reauthentication.

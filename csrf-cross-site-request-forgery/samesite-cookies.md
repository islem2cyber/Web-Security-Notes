# SameSite Cookies

The SameSite attribute controls whether a browser includes a cookie on requests initiated by a cross-site context. It reduces some CSRF exposure but does not replace CSRF protections or define authorization.

## Cookie modes

| Mode | Typical behavior |
|---|---|
| Strict | Sends the cookie primarily in same-site top-level or same-origin-style contexts, subject to browser rules and redirects. |
| Lax | Sends the cookie for normal top-level navigations such as safe link navigation, while withholding it in many cross-site subresource and form contexts. |
| None | Requires `Secure` and allows cross-site use subject to browser third-party-cookie policies. |

Actual behavior depends on browser versions and implementation details.

## Secure and None

The `Secure` attribute requires HTTPS and prevents normal browser transmission over plaintext HTTP. `SameSite=None` indicates that cross-site usage is allowed; it does not mean HTTP is secure, and modern browsers require `Secure` for the None mode.

## Same-origin, same-site, and cross-site

An origin is a scheme, hostname, and port tuple. A site is a scheme and registrable domain concept used for SameSite policy. These concepts are related but not interchangeable.

For example, sibling subdomains may share a site but have different origins. Requests can be same-site while remaining cross-origin, and browser behavior and trust boundaries differ between the cases.

Do not oversimplify a sibling domain as equivalent to a cross-origin or cross-site context. The relationship must be assessed for the cookie and for the browser context being tested.

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

SameSite is valuable defense-in-depth, but browser settings, old clients, embedded contexts, redirects, and application misconfiguration can affect it. Sensitive mutations should still use framework CSRF defenses and explicit intent controls.

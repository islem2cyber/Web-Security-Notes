# Platform Misconfiguration and Access Control Bypass

## Overview

Some access-control weaknesses arise from platform or framework behavior rather than a direct business-logic issue. These may involve URL-based restrictions, HTTP method checks, proxy headers, or front-end and back-end inconsistencies.

## URL-based access control

A common anti-pattern is to protect a resource with a front-end rule or URL pattern while leaving the actual backend path unprotected.

Examples:

- Only admin routes are linked in the UI.
- A frontend route guard hides a page, but the backend action remains callable.
- A server treats `/admin` and `/Admin` differently or parses forwarded values inconsistently.

Such weaknesses may be exploitable when the server-side authorization is absent or incomplete.

## HTTP method-based access control

Applications sometimes restrict `GET` but forget to restrict `POST`, `PUT`, `DELETE`, or `PATCH` on the same action. A direct POST may bypass a UI-only restriction.

Examples:

```http
GET /account/delete/42 HTTP/1.1
```

and

```http
POST /account/delete/42 HTTP/1.1
```

If the second request is not subject to the same authorization policy, the application may be bypassed.

## X-Original-URL and X-Rewrite-URL

Some proxy and web-server configurations use non-standard headers such as `X-Original-URL` or `X-Rewrite-URL` to convey or override a path for downstream request handling. Their meaning is implementation- and configuration-dependent: they are not special routing instructions recognized universally by HTTP servers or frameworks. A reverse proxy may remove, replace, or pass them; a downstream component may ignore or consume them.

An access-control mismatch is possible only when the actual request path and a header-derived path are interpreted differently by relevant layers—for example, if one layer authorizes one path while a trusted downstream component routes another. Assess this only after establishing that the deployed proxy/application chain consumes the header and how it affects routing or authorization. Do not assume that sending either header will change the route.

## Front-end vs back-end inconsistencies

A frequent issue is that the front end restricts access visually, while the backend does not enforce the same rule. Attackers can bypass UI logic by sending crafted requests directly to the endpoint.

This is a practical reminder that the client is not a reliable enforcement point.

## HTTP method override issues

Some frameworks or gateways support method overrides via hidden fields or headers such as:

- `X-HTTP-Method-Override`
- `_method`

If an application accepts these values without validating them properly, a `GET` request may be turned into a privileged action or a route may be reached under an unexpected method.

## Conditions that make these weaknesses exploitable

These issues are exploitable when:

- The authorization policy depends on a route or method check that is not enforced centrally.
- The application trusts request metadata without validating provenance.
- Untrusted headers or rewritten values affect routing or access decisions.
- The backend does not re-check the identity and permissions for the underlying action.

## Defensive requirement

Do not treat headers or method labels as authoritative authorization signals. The server must validate the actual identity, route, and permission policy independently of proxy rewriting and client-controlled metadata.

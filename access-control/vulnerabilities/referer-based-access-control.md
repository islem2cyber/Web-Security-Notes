# Referer-Based Access Control

## What is the Referer header?

The `Referer` request header can contain the URI of the page from which a request was initiated, subject to browser policy and referrer-policy rules. It is not a reliable record of user intent, identity, or privilege, and it is not an authorization credential.

## Why it is attacker-controllable

The header is request metadata supplied by the client: non-browser clients can set or omit it, and browsers may omit or reduce it under policy. While browser JavaScript cannot freely set every restricted request header, a server must not treat a received `Referer` value as proof of authorization. If a control checks for a particular referer value, test its actual behavior in the authorized environment rather than assuming that omission or modification will bypass it.

## Example vulnerability pattern

```http
POST /delete-account HTTP/1.1
Host: example.com
Referer: https://example.com/account/settings
Cookie: session=userA
```

If the server relies on this header instead of checking the authenticated identity's permission for the operation, the authorization decision is based on untrusted metadata. The request example alone does not demonstrate a bypass; the result depends on the application's check and must be established with controlled evidence.

## Why this is insecure

The header may be absent, reduced, or client-supplied. It may be used as a supplementary signal in a broader design, but it must not replace server-side authorization. For request-forgery defenses, use purpose-built protections such as anti-CSRF tokens and appropriate cookie settings; those controls address a different threat and do not grant permission to perform an action.

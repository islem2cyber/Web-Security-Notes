# Parameter-Based Access Control

## Overview

Parameter-based access control occurs when the server trusts a user-controllable value to decide what the user may do. Common sources include URLs, hidden form fields, cookies, and request bodies. This is dangerous because the client is not a trusted authority for authorization policy.

## Examples of attacker-controlled values

- Query parameter: `?role=admin`
- Hidden form field: `<input type="hidden" name="isAdmin" value="true">`
- Cookie: `role=admin`
- JSON body: `{ "admin": true, "userId": 42 }`

If the server uses these values without verifying the authenticated user's actual permissions, the application is vulnerable to parameter tampering.

## Why the server must not trust client state

The server should derive the user's rights from trusted session state, backend identity records, and authorization policy. It should not accept a client-provided role flag as proof of privilege.

A simplistic but common vulnerable pattern is:

```pseudo
if request.params["isAdmin"] == "true":
    allowAdminPanel()
```

This is insecure because any user can send `isAdmin=true` and force the application to make a decision based on untrusted input.

## Defensive design

The correct pattern is:

```pseudo
user = getCurrentUserFromSession()
if user.role != "admin":
    deny()
```

The server uses trusted session and database state, not user-controlled values, to make the decision.

## Role and privilege parameters are not trust anchors

If the application stores authorization data in a hidden field, cookie, or request parameter, those values are not authoritative. They may be modified by the attacker or replayed in another context. Authorization decisions must be rooted in a backend policy that the client cannot change without a valid authenticated session and a server-side check.

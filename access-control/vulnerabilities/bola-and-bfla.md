# BOLA and BFLA

## Overview

BOLA and BFLA are API-focused terminology for broken access control. They describe situations in which an API fails to enforce object-level or function-level authorization.

## BOLA — Broken Object Level Authorization

BOLA describes authorization failures that occur on the object level. An attacker makes a request against an object they should not access, such as another user's account, file, order, or document.

Typical patterns:

- `GET /api/customers/42` returns another user's customer record.
- `PATCH /api/invoices/99` modifies a different invoice.
- `DELETE /api/files/abc123` deletes a file the user does not own.

This is closely related to IDOR. IDOR traditionally describes the manipulable direct reference, while BOLA names the failure to enforce authorization for the requested API object. The terms overlap, and neither is defined solely by whether an identifier is predictable or directly visible.

## BFLA — Broken Function Level Authorization

BFLA describes failures where the server allows a user to call a function or operation they should not be able to access. This is usually a function-level permission issue rather than a direct object access issue.

Examples:

- A normal user calls `DELETE /api/users/5`.
- A low-privilege account triggers a privileged `export` or `approve` endpoint.
- A user invokes an admin-only API action by guessing or inferring the route.

BFLA concerns unauthorized access to functions or operations, regardless of the object being acted on.

## Relationship to IDOR and broken access control

BOLA and IDOR overlap and are often used interchangeably; the distinction is primarily one of terminology and emphasis:

- IDOR traditionally highlights a user-controllable direct reference to an object.
- BOLA emphasizes the missing object-level authorization check, commonly in an API.

The presence of an object identifier or a changed response is not by itself a finding. Confirm that the authenticated caller can access or act on an object the applicable policy does not permit.

BFLA is similarly related to broken access control, but it focuses on the action or endpoint rather than the object itself.

## Why these are especially important in APIs

Modern web APIs frequently expose a large number of object-based endpoints. The application may rely on client-supplied identifiers and route patterns. If each endpoint does not enforce object ownership and function permissions, the API may inadvertently become an authorization bypass surface.

### Example request

```http
GET /api/users/7 HTTP/1.1
Host: example.com
Authorization: Bearer <token>
```

If the token belongs to user 2 but the endpoint returns user 7's record, then the API is vulnerable to BOLA.

### Function-level example

```http
POST /api/admin/export-report HTTP/1.1
Host: example.com
Authorization: Bearer <standard-user-token>
```

If the server permits this call without checking the user's role, it is vulnerable to BFLA.

## Connection to future API Security work

This material connects directly to the future API Security stage of the roadmap. The same lessons apply to REST, JSON-based APIs, microservices, and serverless endpoints that rely on tokens, scopes, and permissions. The core principle is unchanged: access decisions must be enforced server-side on the user identity, the requested object, the action, and the current authorization policy.

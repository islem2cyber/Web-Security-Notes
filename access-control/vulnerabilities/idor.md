# IDOR — Insecure Direct Object References

## Definition

IDOR is an access-control weakness in which a user can manipulate a reference to an object or resource in order to access data or actions that should be protected by authorization. The issue often appears when the application uses client-controlled identifiers to locate objects without enforcing ownership or role checks.

## Root cause

The root cause is a mismatch between trust boundaries: the server accepts a user-supplied identifier as the selector for a resource while failing to validate whether the current user is authorized to access that resource.

## Direct object references

A direct object reference is any identifier or pointer that maps directly to a resource, such as:

- Account IDs.
- File IDs.
- Invoice numbers.
- Order identifiers.
- Usernames.
- Document slugs.
- Database primary keys.

These values are often passed in URLs, query strings, JSON bodies, cookies, or headers.

## Predictable vs unpredictable identifiers

Predictable identifiers make exploitation easier, but unpredictability alone does not eliminate the vulnerability. A random or non-sequential ID can still be vulnerable if the application fails to check authorization.

Examples:

- Predictable: `/invoice?id=55`
- Less predictable: `/orders/8a7b3c9e-1121-4b12-b4d0-7d1f2e550ea7`

The vulnerability is not the ID format itself. The vulnerability is whether the application validates access before returning or modifying the target object.

## IDOR in different locations

IDOR can appear wherever the application accepts an identifier:

- URL path: `/user/123/profile`
- Query string: `/invoice?accountId=123`
- JSON body: `{ "userId": 123 }`
- Header: `X-Account-ID: 123`
- Cookie: `account_id=123`

A vulnerable application may accept any of these as a selector without verifying authorization.

## Read-based IDOR

The user changes a user identifier and accesses another user's data.

Example:

```http
GET /account?id=15 HTTP/1.1
Cookie: session=userA
```

If `id=15` corresponds to another user and the server returns that user's profile, the issue is a read-based IDOR.

## Write-based IDOR

A user modifies a resource owned by someone else.

Example:

```http
POST /profile/update HTTP/1.1
Cookie: session=userA
Content-Type: application/x-www-form-urlencoded

userId=23&email=attacker@example.com
```

If the application updates user 23's profile, that is a write-based IDOR.

## Delete-based IDOR

The user submits a request that deletes or disables another person's resource.

Example:

```http
POST /delete-comment HTTP/1.1
Cookie: session=userA
Content-Type: application/x-www-form-urlencoded

commentId=993
```

A successful deletion of someone else's comment without the required ownership or privilege check is a delete-based IDOR.

## Function and action-based IDOR

Sometimes the object is not a record but an action or state toggle. A resource may be passed as a token or operation ID that triggers a privileged action.

Examples:

- `action=disableUser`
- `approveRequestId=987`
- `transferId=455` with a different or privileged target

If the attacker can trigger an action on another resource without authorization, it is still a broken access-control issue.

## Horizontal and vertical IDOR

- Horizontal IDOR: user A accesses user B's same-level resource.
- Vertical IDOR: user A accesses admin or higher-privilege resource.

A single weak pattern may enable both if administrative records are selected by ID and the server fails to check role boundaries.

## IDOR and broken access control

IDOR is one of the clearest forms of broken access control. The application is effectively trusting a caller-controlled identifier as if it already implies authorization. The essential question is not whether the identifier is valid, but whether access is authorized.

## IDOR and BOLA

IDOR and BOLA describe closely related authorization failures, and the terms are sometimes used interchangeably. IDOR traditionally emphasizes a reference to an object that a requester can manipulate; BOLA (Broken Object Level Authorization) is API-security terminology that emphasizes the missing authorization check on the requested object. Neither term requires a particular identifier format or transport. An API request can exhibit both characteristics, and merely observing a direct reference does not establish either vulnerability: unauthorized object access or action must be demonstrated against the intended policy.

See [BOLA and BFLA](./bola-and-bfla.md) for the API object-level and function-level distinction.

## Why changing an identifier is not automatically a vulnerability

An identifier change alone is not proof of a flaw. The critical question is whether the server demonstrates the required authorization check. Many applications use IDs legitimately. The vulnerability is only demonstrated when the user can access an unauthorized object or action under a controlled test.

## Ownership and permission checks

Proper enforcement should check:

- Is the requested object valid?
- Does the current user own it?
- Is the current user in an authorized role?
- Is the requested action allowed in the current workflow or state?
- Are there any records or permissions that override the direct reference?

The authorization decision must be server-side and based on trustable state, not simply the presence of a selector value.

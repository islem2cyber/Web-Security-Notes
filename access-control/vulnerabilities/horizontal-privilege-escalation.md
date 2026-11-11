# Horizontal Privilege Escalation

## Definition

Horizontal privilege escalation occurs when a user accesses another user's resources or actions at the same privilege level. This is often the result of poor ownership checks, predictable identifiers, or insufficient validation of per-user scope.

## Common patterns

- User A views user B's account profile.
- User A edits another user's invoice.
- User A deletes another user's uploaded file.
- User A changes the settings of another account without a permission boundary.

## User-controlled identifiers

A common root cause is that the application uses a user-controlled identifier such as a numeric ID, UUID, or slug to select the targeted object. If the server fails to verify that the object belongs to the authenticated user or the role is allowed to act on it, unauthorized access may occur.

Example:

```http
GET /account?id=4721 HTTP/1.1
Host: example.com
Cookie: session=userA_session
```

If the server returns another user's details for `id=4721`, the issue is a horizontal access-control failure.

## ID manipulation and predictable identifiers

Predictable numeric IDs and GUIDs are common in insecure designs. A user may iterate over IDs or modify the parameter value until they find another record. Predictability alone is not automatically a vulnerability, but it makes exploitation easier when ownership checks are absent or weak.

## Information disclosure through other features

Horizontal escalation can also occur through features that reveal other users' data by accident, such as:

- Search results exposing personal records.
- Error messages revealing record ownership.
- Export functions returning records outside the user's scope.
- APIs that filter only by user input instead of server-side policy.

## Read, update, delete, and action-level failures

Horizontal escalation is not limited to read access. It may also include:

- Updating another user's profile.
- Canceling their subscription.
- Deleting a record.
- Performing actions, such as approve or reassign, on objects they do not own.

## Escalation from horizontal to vertical

A horizontal flaw may become a vertical one if an attacker gains access to a privileged user account or privileged object. For example, compromising one admin's account or obtaining a privileged resource could let the attacker access other admin-only functions. The impact then crosses privilege boundaries.

## Defensive requirement

The server must tie the requested resource to the current authenticated identity and policy. Access decisions should consider:

- Ownership.
- Role.
- Resource state.
- Workflow conditions.
- Whether the operation is allowed for this user on this object.

## Example access-control check

```pseudo
user = getCurrentUserFromSession()
resource = getResourceById(request.id)
if resource.ownerId != user.id and !user.hasRole("admin"):
    deny()
```

This is the type of server-side policy the application must enforce.

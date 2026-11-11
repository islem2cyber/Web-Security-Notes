# Secure Access Control Design

## Design principles

Secure access control begins with a clear authorization model. Applications should define which roles, groups, and users may access which resources and actions, and then enforce that policy everywhere the server processes requests.

## Key recommendations

- Enforce authorization server-side for every sensitive route and action.
- Use least privilege and deny-by-default policy design.
- Base decisions on trusted session state and backend policy instead of client-controlled values.
- Validate ownership and role at the point of access, not merely during page rendering.
- Apply checks to all relevant requests, including APIs and indirect or alternate workflows.
- Treat hidden links and random URLs as convenience features, not protections.
- Validate workflow state and resource context at each sensitive step.
- Centralize permission checks to reduce drift and inconsistent patterns.

## Authorization architecture

A robust design typically includes:

- A service or policy layer for deciding whether access is allowed.
- A resource ownership model that ties records to users or groups.
- Role-aware or attribute-aware enforcement.
- Clear separation between authentication and authorization logic.
- Validation of workflow state before any mutating action.

## Example enforcement pattern

```pseudo
function canUserAccess(user, resource, action): bool
    if user is null:
        return false
    if user.role == "admin":
        return true
    if action == "view" and resource.ownerId == user.id:
        return true
    return false
```

This pattern makes the authorization decision explicit, testable, and independent of client-controlled values.

## Why policy should be centralized

When authorization logic is spread across templates, controllers, and client-side scripts, it becomes harder to reason about and easier to bypass. Centralizing policy makes reviews and tests more predictable and reduces the chance of one route accidentally skipping the required check.

## Final principle

An application is not secure simply because a user can log in, or because a URL is hidden, or because a UI element is absent. A secure application is one where the server consistently enforces the correct policy for each authenticated identity, each resource, and each action in every relevant context.

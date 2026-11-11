# Access Control Models

Access control models provide different ways to structure policy and enforcement. No single model is required for every application, but understanding the common patterns helps explain why authorization logic may be correct in one context and weak in another.

## RBAC — Role-Based Access Control

RBAC assigns permissions to roles, and roles to users. This model is common in enterprise software, admin tooling, and application backends.

Example:

- `guest` may read public information.
- `editor` may create or edit content.
- `admin` may manage users and system settings.

This is simple and easy to reason about, but it can become rigid when fine-grained permission boundaries are needed. Users with the same role receive the same access even if different resource ownership or context matters.

## ABAC — Attribute-Based Access Control

ABAC evaluates access based on subject, object, environment, and action attributes. This model can express more granular rules than RBAC.

Example policy:

- A manager may approve expense claims only for their own department.
- A user may read documents marked `confidential` only if their `clearance` attribute satisfies the policy.
- A service account may access a data export endpoint only during business hours.

ABAC is powerful when access depends on many conditions, but it can be more complex to implement and audit.

## DAC — Discretionary Access Control

DAC allows owners or creators of objects to grant or restrict access to others. The object owner can decide permissions.

Example:

- A user owns a private note and controls who sees it.
- A document owner can grant view or edit access to teammates.

DAC is common in file sharing, personal account systems, and collaborative tools. The challenge is that ownership decisions are often implemented inconsistently or exposed to untrusted client input.

## MAC — Mandatory Access Control

MAC enforces a centrally defined policy that users cannot override. It is common in high-assurance systems where classification levels and constraints are mandated by the environment.

Example:

- Confidential documents are not readable by unclassified users.
- A lower-security process cannot write to a higher-security object.

MAC is not typically the primary model in mainstream web applications, but it appears in secure enterprise and military-style systems.

## Practical web application examples

| Model | Typical pattern | Example |
|---|---|---|
| RBAC | Roles determine access | Admins access user management pages. |
| ABAC | Rules based on attributes | Only managers in the same department can approve a request. |
| DAC | Object owner grants permissions | A user shares a file with a collaborator. |
| MAC | Central security policy is enforced | Secret data cannot be read by a lower-clearance user. |

## Why models matter in web security

Understanding the model helps assess whether the application is using a trusted, central policy or if it is relying on a fragile, user-controlled signal. Many access-control flaws arise when the app makes decisions based on a client submission, weak guessable parameter, or UI-only role state rather than the actual policy attached to the authenticated identity.

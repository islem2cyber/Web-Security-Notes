# Access Control Fundamentals

## What is access control?

Access control is the decision-making process that determines whether an identity is allowed to do something with a resource. In web applications, the resource might be a document, account profile, administrative page, API action, or configuration object. The access decision is usually based on authorization policy, the authenticated identity, the current session context, and the state of the resource.

A secure application does not simply ask, “Is the user logged in?” It asks, “Does this authenticated identity have the required permission for this specific action on this specific resource in this specific context?”

## Authentication, session management, and authorization

These concepts are related but distinct:

- Authentication answers: “Who is this user?”
- Session management answers: “What state is this user currently in?”
- Authorization answers: “Is this user allowed to do this?”

Users may authenticate successfully yet still be denied access. Authentication without effective authorization is not a safe security boundary.

## Why authentication does not guarantee authorization

Authentication proves identity only at a point in time. It does not guarantee that the user may access all functions or objects that happen to exist in the application. Authorization policies encode the intended boundaries: whether a user is a normal user, admin, owner, or member of a specific group; whether a record belongs to that user; whether a workflow is valid in the current state.

A server must enforce those boundaries consistently. If it trusts client-controlled values, skipped steps, or incomplete checks, the application may grant access beyond the intended policy.

## Core concepts

### Resources

A resource is anything protected by authorization: a user record, page, API action, settings panel, document, payment, order, report, or configuration object.

### Actions

An action is the operation on a resource: read, update, delete, approve, create, export, reset, or escalate privileges.

### Identities

An identity is the authenticated principal or subject. In web apps, this might be a username, account ID, session, token subject, or linked directory identity.

### Roles and permissions

A role is a grouping of permissions. A permission is a rule that allows or denies a specific action on a resource or class of resources. Authorization logic may use any combination of roles, ownership, time windows, resource state, and policy rules.

### Policies

Policies define the allowed conditions for access. They often include:

- User or group membership.
- Resource ownership.
- Role or privilege level.
- Temporal or state-based constraints.
- Administrative approval workflows.

## Key principles

### Least privilege

Users should receive only the permissions needed to perform their role or task. Excess privileges increase the blast radius of compromise and weaken the security boundary.

### Deny by default

The default should be to deny access unless the application has an explicit, validated reason to allow it. Hidden links, guessed routes, or client-side checks should not grant access.

### Defense in depth

Authorization should not rely on one layer alone. Front-end validation, URL-based restrictions, and UI hiding are useful convenience measures, but they do not replace server-side enforcement.

### Server-side vs client-side authorization

Client-side checks can improve the user experience, but they are not a trust boundary. Attackers can manipulate the client or direct requests to server endpoints. Access control must be enforced on the server for every sensitive action.

### Resource ownership and authorization

Many vulnerabilities arise when the application checks only “Does the requester know a valid identifier?” instead of “Does this requester own or otherwise have permission over this resource?” Ownership must be validated server-side using trusted state.

## Horizontal, vertical, and context-dependent access control

- Horizontal privilege escalation: user A accesses user B's resources at the same privilege level.
- Vertical privilege escalation: user A gains a higher privilege level or administrative capability.
- Context-dependent access control: a user can perform an action only in certain states, contexts, or workflow conditions. A bypass occurs when those conditions are not validated at every sensitive step.

These categories are not mutually exclusive. A horizontal IDOR issue may also be used to reach a privileged account if the attacker obtains a resource owned by an admin or a privileged workflow.

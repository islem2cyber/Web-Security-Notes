# Authentication vs Authorization

Authentication and authorization are often discussed together, but they address different questions.

## Authentication

Authentication verifies the identity presented by the user. It answers: “Who is this person or system?”

Common mechanisms include:

- Username and password.
- MFA or OTP checks.
- Cookies or session identifiers.
- SSO assertions or tokens.

A successful login or token validation creates an identity with some level of trust, but it does not mean the user is authorized to every resource.

## Authorization

Authorization is the decision that determines whether the authenticated identity may access or act on a resource. It answers: “Is this user allowed to do this here?”

A permission check might involve:

- Role membership.
- Resource ownership.
- Policy-based conditions.
- Business restrictions or workflow state.

## Why authorization must be evaluated repeatedly

A user who is authenticated at one time may not have permission at another time. Access can depend on:

- Role assignments.
- Account state.
- Session validity.
- MFA completion.
- Resource ownership.
- The current step in a multi-step workflow.

## Common confusion

Many applications assume that login success implies access to all pages or operations with the same namespace. This is a misconception. A user may be logged in and still be forbidden from modifying admin settings, reading another user's account, or approving a workflow that requires a different role.

## Example

A user logs into an application and receives a session cookie. The session proves the user exists and is authenticated. But if the application exposes `/admin/users` without verifying the session role, the user can access administrative functionality even though the intended policy denies it. In this case, the vulnerability is authorization failure, not authentication failure.

## Defensive principle

The server must evaluate authorization for every sensitive page, action, and API route using trusted server-side state. The presence of a valid session is necessary but not sufficient.

# Multi-Step Process Vulnerabilities

## Overview

Multi-step processes are common in admin workflows, order approvals, password change flows, billing updates, and account recovery. A vulnerability occurs when the application enforces authorization only at some steps and not at others.

## Typical issue pattern

A workflow includes several steps:

1. Request a change or action.
2. Confirm the request.
3. Submit the final mutation.
4. Commit the resulting state.

If the authorization checks happen only in step 1 or on the front-end form, an attacker may bypass the earlier validation by directly submitting the final step or skipping the intended intermediate validation.

## Direct submission of a later-stage request

An attacker may send a request directly to a later stage without completing the earlier workflow. This may happen when:

- The front-end hides the earlier steps.
- The server reuses stale state objects.
- The server checks role only once, before the final action.
- The final action trusts a user-controlled workflow ID or token.

## Workflow state validation

A secure application must validate the current state of the workflow and ensure that the user has permission at the time of each sensitive action.

Examples:

- A user cannot approve an invoice without the appropriate role.
- A change request cannot be submitted unless the current step matches the server-side state.
- A resource update must fail if the workflow state no longer permits it.

## Example

An admin panel lets a user create a role change request in one step and submit it in another step.

```http
POST /admin/change-role HTTP/1.1
Cookie: session=userA

userId=42&newRole=admin
```

If the server accepts this request without confirming that the current authenticated user has admin rights at the moment of submission, the workflow is vulnerable.

## Defensive principle

Authorization should be enforced at every sensitive step and based on the current identity, session, and workflow state. A server must not assume that a user safely reached the final step through the intended UI path.

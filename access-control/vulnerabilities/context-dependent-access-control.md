# Context-Dependent Access Control

## Overview

Context-dependent access control means the authorization decision depends on environment, timing, workflow, or surrounding conditions. For example, access may be allowed only during a specific state, for a particular account, or after a previous action.

Examples include:

- Users may edit a resource only while it is in draft state.
- An MFA challenge may only be accepted for the same session and identity.
- Admin actions may be restricted to certain IP ranges, departments, or approved workflows.

## Failure modes

These controls are often bypassed when the server checks the conditions too early, too late, or only in one step. A user may pass a request that should be invalid in the current state because the application does not verify the state at the moment the sensitive action occurs.

## Example

A user creates a ticket and then later submits a final status change request without verifying that their ticket is still in the allowed state. If the server does not re-check the current state, the action may be applied even though the workflow requires a different state or role.

## Defensive principle

Access decisions must be repeated and validated at the point of execution using the actual current state. The server should not rely on a previously displayed UI state, a stale token, or an untrusted client-supplied context.

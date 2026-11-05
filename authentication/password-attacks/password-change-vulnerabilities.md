# Password-Change Vulnerabilities

Password change is a sensitive authenticated workflow. It should verify that the request belongs to the intended user, require appropriate proof of control, and apply protections to every credential-checking step.

## Secure workflow properties

1. Derive the target identity from the authenticated server-side session, not a caller-controlled account name or identifier.
2. Require the current password or an appropriately recent re-authentication for a sensitive change, according to product risk.
3. Validate the current credential on the server and bind the result to the same user and transaction as the proposed new password.
4. Apply rate limits and monitoring to current-password checks; do not expose distinct account-validity information unnecessarily.
5. Validate and safely store the new password using the application's approved password policy and password-hashing scheme.
6. Notify the user through a trusted channel and consider invalidating other sessions or refresh tokens after the change.
7. Use CSRF protections where browser cookies authenticate the change request, and avoid leaking secrets through logs or analytics.

## Common implementation errors

- Trusting an account identifier in the request instead of the authenticated principal.
- Checking the current password in one step but failing to bind it to the later update.
- Omitting throttling because the endpoint is considered “already authenticated.”
- Applying limits only to one route or request shape.
- Updating the password before all required checks have succeeded.
- Leaving old persistent sessions valid when product policy expects them to be revoked.

The exact controls depend on the product's authentication model, but authorization must be checked server-side for every state-changing request. Authentication proves identity; it does not automatically authorize changes to an arbitrary account.

## Completed lab

**Password brute-force via password change** is completed. It demonstrates that password-change workflows can expose a credential-verification surface and must not be excluded from rate-limit analysis. The lab status is recorded in the [tracker](../lab-tracker.md).

For general controls and safe testing boundaries, see [brute-force protection](./brute-force.md) and the [discovery workflow](../methodology/discovery-workflow.md).

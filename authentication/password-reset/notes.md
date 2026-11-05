# Password Reset and Account Recovery

Password reset is an authentication workflow that lets a user regain access without the current password. It must establish the account identity safely and provide a narrowly scoped, temporary authorization to change that account's password.

## Reset workflow and token properties

A typical flow accepts a recovery identifier, sends a recovery mechanism through a trusted channel, verifies a reset token, and permits a password change. Each step should remain bound to the same account and recovery transaction.

Reset tokens should be generated with a cryptographically secure random source and sufficient entropy for the threat model. They should be unpredictable, scoped to one user and purpose, expire in a reasonable period, be single-use, and be invalidated after successful reset or a relevant account/security change. Store tokens safely, avoid placing them in logs or analytics, and protect the reset operation against CSRF and session confusion where applicable.

## Identity binding and link construction

Build reset links from a configured, trusted canonical origin rather than blindly trusting request-supplied host or forwarding headers. Middleware, proxies, and frameworks may transform these headers; validate trusted proxy configuration and allowlist expected hostnames at the appropriate boundary.

If unvalidated request headers influence the generated link, an attacker may cause a legitimate reset message to contain a link pointing to an unintended host. That is a **reset-link construction** weakness: the token may be valid but delivered in a poisoned destination. This differs from a **reset workflow** weakness, where token verification, identity binding, expiry, reuse, or password-change authorization is flawed.

## Enumeration and recovery UX

Different responses for registered and unregistered identifiers can disclose account existence. Use a consistent public response where appropriate, while still delivering actionable information to the account owner through the trusted recovery channel. Consider timing, status, redirects, and follow-up behavior as well as message text.

Recovery should not become a weaker alternative to MFA or ordinary authentication. Factor replacement and support-assisted recovery require clear assurance requirements, auditability, and user notifications.

## Secure implementation checklist

- Generate high-entropy tokens with a vetted cryptographic random generator.
- Bind each token to one account and one purpose; validate it server-side.
- Apply expiry, single-use semantics, and invalidation after use.
- Construct links from trusted configuration and validated host information.
- Avoid account-existence disclosure in unauthenticated responses where appropriate.
- Rate-limit issuance and verification without creating easy denial-of-service conditions.
- Invalidate or re-evaluate sessions and persistent tokens after a password reset.
- Redact tokens from logs, referrers, analytics, and error reports.
- Notify users of reset completion and provide a safe way to report unexpected changes.

## Completed labs

- **Password reset broken logic** — illustrates flaws in reset workflow logic and identity handling.
- **Password reset poisoning via middleware** — illustrates the risk of trusting unvalidated request headers during reset-link construction.

These are the recorded Academy lab topics, not claims about a particular real application's implementation. See the [lab tracker](../lab-tracker.md) and [discovery workflow](../methodology/discovery-workflow.md).

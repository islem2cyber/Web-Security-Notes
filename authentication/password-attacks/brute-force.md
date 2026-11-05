# Brute-Force Protection

Online brute force is repeated password guessing against an authentication service. The security question is whether the application can limit and detect unauthorized verification attempts while preserving availability for legitimate users.

## Mechanics and defensive assumptions

A service may apply throttling per account, source address, device signal, session, or combinations of these. Any one identifier can be shared, changed, or spoofed in some environments; a single IP-only block is therefore not a complete account-abuse control. Distributed activity can also evade simplistic source-based thresholds. Defenses should combine signals rather than rely on an assumption that all attempts come from one address.

Account lockout can slow guessing, but an attacker may abuse it to prevent legitimate sign-in. Prefer carefully designed progressive delays, risk-based challenges, and recovery paths over a permanent lock that an unauthenticated party can trigger trivially. Make throttling consistent across all credential-verification routes.

## Implementation and monitoring

- Enforce limits server-side and apply them to the logical authentication attempt, including alternate request formats or workflows.
- Use layered per-account and aggregate controls. Avoid exposing account validity through the selected limit or lockout response.
- Apply controls to login, MFA verification, password change, and recovery where they verify secrets.
- Monitor patterns across accounts, sources, devices, and time; alert on unusual failure rates and sudden distributed changes.
- Protect legitimate users with clear recovery and support processes. Avoid logging plaintext passwords or OTPs.
- Treat unusual request shapes as an implementation concern: validation and throttling must still run before credentials are accepted.

## Completed lab concepts

- **Broken brute-force protection, IP block:** demonstrates why a control based only on a network address can be inadequate when its assumptions do not hold.
- **Broken brute-force protection, multiple credentials per request:** demonstrates the importance of validating and throttling the actual authentication operation, not merely one expected request representation.
- **Password brute-force via password change:** illustrates why secondary credential-verification flows need their own effective protections. See [password-change vulnerabilities](./password-change-vulnerabilities.md).

These summaries describe the security concepts behind completed Academy exercises, not universal procedures or instructions for attacking real accounts. The [lab tracker](../lab-tracker.md) records exact statuses.

## Safe assessment

Only assess rate limiting in an explicitly authorized environment with test accounts and an agreed request budget. Use a small, bounded set of attempts, monitor the effect, and stop before causing lockouts or service degradation. Record the observed scope, threshold behavior, reset behavior, and whether retries are handled consistently. Do not test guessed credentials against real users.

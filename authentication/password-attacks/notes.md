# Password Attacks and Weaknesses

Password security depends on more than password length or a login endpoint. The application must safely verify credentials, protect stored verifiers, resist automated guessing, and avoid weakening controls in adjacent workflows such as password change and recovery.

## Common weaknesses

- **Weak password policies:** Policies that permit common or previously exposed passwords increase guessing risk. Effective policy should support long unique passwords and screen against known-compromised choices without relying on arbitrary composition rules alone.
- **Brute force:** Repeated guesses against one account or a small set of accounts. Risk depends on password strength and effective online controls.
- **Credential stuffing:** Reuse of username/password pairs exposed in breaches of other services. It relies on password reuse, not guessing each password from scratch.
- **Password spraying:** A small number of likely passwords tried across many accounts. It seeks to avoid per-account thresholds and requires detection across accounts and sources.
- **Password reuse:** A user's valid password on one service can put other accounts at risk after a breach.
- **Insufficient rate limiting or weak lockout:** Controls may be absent, inconsistently applied, scoped only to one IP, or vulnerable to denial-of-service tradeoffs.
- **Password-change weaknesses:** A change endpoint may fail to verify the current user or current credential, bind the change to the authenticated identity, or protect verification attempts. See [password-change vulnerabilities](./password-change-vulnerabilities.md).

## Distinguishing online guessing patterns

| Pattern | What is reused | Typical focus | Defensive emphasis |
|---|---|---|---|
| Brute force | One account; many candidate passwords | Guessing a password for a specific identity | Per-account throttling, progressive friction, strong passwords, anomaly detection. |
| Credential stuffing | Credential pairs from another breach | Testing password reuse across services | Breached-password screening, risk-based controls, MFA, abuse detection across accounts and sources. |
| Password spraying | A few likely passwords across many accounts | Avoiding per-account thresholds | Cross-account and cross-source detection, sensible global limits, MFA, user notifications. |

These descriptions characterize patterns, not a prescribed testing procedure. Different products and threat models require different signals and controls.

## Defenses in layers

Use strong password hashing for stored credentials, support unique passwords and password managers, offer MFA, and apply server-side throttling that accounts for multiple identifiers and network sources. Monitor both per-account and aggregate patterns. Design lockout carefully: permanent or easily triggered lockouts can let an unauthenticated party deny service to a user.

Rate limits should be enforced by the server and apply to the logical verification attempt, not merely a particular request shape. Recovery and password-change paths need protections comparable to login. See [brute-force concepts](./brute-force.md), [password storage](./password-cracking.md), and [MFA](../multi-factor-authentication/notes.md).

The related completed labs are listed in the [lab tracker](../lab-tracker.md). Lab results are evidence of learning exercises, not a claim that the same behavior is universal.

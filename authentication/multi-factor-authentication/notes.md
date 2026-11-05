# Multi-Factor Authentication

Multi-factor authentication (MFA) verifies an identity using more than one independent category of evidence. Two-factor authentication (2FA) is MFA with exactly two factors; “two steps” or two checks do not necessarily mean two distinct factors.

## Factor categories and OTPs

- **Something you know:** for example, a password or PIN.
- **Something you have:** for example, a registered authenticator device or security key.
- **Something you are:** a biometric characteristic, handled with care because it cannot be changed like a password.

One-time passwords (OTPs) are commonly used with an authenticator or token. **TOTP** derives a short-lived code from a shared secret and a time step. **HOTP** derives codes from a shared secret and counter. OTP codes are short-lived or counter-based verification values, not by themselves a complete MFA design. They must be bound to the correct account and challenge, protected from replay as appropriate, and rate-limited.

## Enrollment and verification lifecycle

A secure implementation handles factor enrollment, confirmation, challenge creation, verification, recovery, removal, and replacement. Enrollment should require an authenticated context and appropriate proof; new factors should not silently replace established ones. Recovery paths are part of the MFA security boundary and need comparable identity assurance.

### Authentication states

- **Before 2FA:** primary credentials may have been accepted, but the user has not completed the second factor.
- **Partially authenticated:** the application may hold a limited, challenge-specific state that permits only completion or cancellation of the challenge.
- **After successful 2FA:** the server verifies the correct factor for the same user and promotes the session to the fully authenticated state.
- **Fully authenticated:** subsequent authorization is based on server-side identity and policy, not simply on a client-supplied “MFA complete” flag.

The transition must be explicit, identity-bound, and enforced on every protected endpoint. A valid primary login must not accidentally grant access to functions reserved for a fully authenticated session.

## Common failure modes

- **Incomplete verification:** some routes accept primary credentials as if the whole MFA flow had completed.
- **Weak state transitions:** challenge state can be skipped, replayed, or confused with a fully authenticated session.
- **Identity inconsistency:** the account established by the first factor differs from the identity checked during second-factor verification.
- **Insufficient OTP throttling:** repeated guesses are not bounded at the account and challenge level.
- **Unsafe recovery:** recovery codes, factor reset, or support flows bypass the assurance of the enrolled factor.
- **Session mistakes:** a pre-authentication session is reused without safe rotation or a partially authenticated session is accepted for privileged actions.

## Defenses

Bind each challenge and factor to the server-side user and session; expire challenges; prevent unintended reuse; apply layered retry limits and monitoring; rotate session identifiers at authentication boundaries; and invalidate or appropriately re-evaluate sessions when factors change. Store recovery codes securely, make them single-use where designed, and provide safe recovery without making it an easier bypass than the primary flow.

## Completed and postponed labs

- **Completed:** 2FA simple bypass.
- **Completed:** 2FA broken logic.
- **Postponed intentionally:** 2FA bypass using a brute-force attack (Expert).

The postponed lab is not solved or failed. It is reserved for Practical Pentesting training because it requires more advanced Burp Suite Intruder configuration, particularly [Macros and session handling](../burp-suite/macros-and-session-handling.md). The exact statuses are in the [lab tracker](../lab-tracker.md).

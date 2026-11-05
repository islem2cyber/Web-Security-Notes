# Authentication Vulnerabilities

Personal notes and PortSwigger Web Security Academy lab progress from my study of authentication security. This section documents completed lab experience alongside concepts and tools that remain learning references; it does not claim mastery of authentication security.

## Scope

Authentication establishes or verifies an identity. Authorization determines what an authenticated identity is allowed to do. Authentication security includes credential verification, multi-factor authentication (MFA), account recovery, and the sessions that represent an authenticated user.

These notes focus on common weaknesses and defensive reasoning:

- Username enumeration and authentication feedback.
- Password attacks and weak brute-force defenses.
- MFA workflow and state-transition errors.
- Password reset and account recovery.
- Session management and persistent sign-in.
- Safe, repeatable discovery in authorized environments.

## Repository structure

- [Lab tracker](./lab-tracker.md)
- [Discovery workflow](./methodology/discovery-workflow.md)
- [Username enumeration](./username-enumeration/notes.md)
- Password attacks: [overview](./password-attacks/notes.md), [brute force](./password-attacks/brute-force.md), [password cracking](./password-attacks/password-cracking.md), [password-change vulnerabilities](./password-attacks/password-change-vulnerabilities.md)
- [Multi-factor authentication](./multi-factor-authentication/notes.md)
- [Password reset](./password-reset/notes.md)
- [Session management](./session-management/notes.md)
- Burp Suite: [overview](./burp-suite/notes.md), [Intruder](./burp-suite/intruder.md), [Macros and session handling](./burp-suite/macros-and-session-handling.md)
- [Authentication exploitation and impact](./exploitation/notes.md)

## Lab progress

There are **13 completed labs and 1 intentionally postponed lab**. The postponed Expert lab, “2FA bypass using a brute-force attack,” is reserved for later Practical Pentesting training on advanced Burp Suite Intruder Macros and session handling. It is not a failure. See the [complete tracker](./lab-tracker.md).

## Recommended study order

1. Review the [discovery workflow](./methodology/discovery-workflow.md) and [username enumeration](./username-enumeration/notes.md).
2. Study [password attacks](./password-attacks/notes.md), then the [brute-force](./password-attacks/brute-force.md), [password storage](./password-attacks/password-cracking.md), and [password-change](./password-attacks/password-change-vulnerabilities.md) notes.
3. Follow the [MFA](./multi-factor-authentication/notes.md), [password reset](./password-reset/notes.md), and [session management](./session-management/notes.md) workflows.
4. Use the [Burp Suite reference](./burp-suite/notes.md) alongside authorized labs; revisit its [Macros and session handling guide](./burp-suite/macros-and-session-handling.md) during future practical training.
5. Review [impact and exploitation concepts](./exploitation/notes.md), keeping lab evidence distinct from general risks.

## Future learning objectives

Continue practicing systematic, evidence-based discovery and defensive analysis. During the later Practical Pentesting stage, learn advanced Burp Suite Intruder Macros and session handling, then revisit the postponed 2FA lab in its authorized Academy environment. Topics documented here are not all mastered skills.

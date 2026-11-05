# Authentication Lab Tracker

This tracker records the verified PortSwigger Web Security Academy progress provided for this learning path. “Postponed” means intentionally deferred for later study, not failed.

## Labs

| # | Lab name | Difficulty | Status | Related topic | Key learning objective |
|---:|---|---|---|---|---|
| 1 | Username enumeration via different responses | Apprentice | Completed | Username enumeration | Recognize explicit differences in authentication feedback. |
| 2 | 2FA simple bypass | Apprentice | Completed | MFA | Understand why enforcing the complete verification flow matters. |
| 3 | Password reset broken logic | Apprentice | Completed | Password reset | Examine identity binding and reset-workflow logic. |
| 4 | Username enumeration via subtly different responses | Practitioner | Completed | Username enumeration | Compare response behavior beyond obvious error text. |
| 5 | Username enumeration via response timing | Practitioner | Completed | Username enumeration | Consider repeatable timing differences as a side channel. |
| 6 | Broken brute-force protection, IP block | Practitioner | Completed | Brute-force defenses | Understand limitations of relying on an IP block alone. |
| 7 | Username enumeration via account lock | Practitioner | Completed | Username enumeration | Assess how lockout behavior can disclose account validity. |
| 8 | 2FA broken logic | Practitioner | Completed | MFA | Analyze consistency between identity, session, and second-factor checks. |
| 9 | Brute-forcing a stay-logged-in cookie | Practitioner | Completed | Session management | Understand risks from weak persistent-authentication token construction. |
| 10 | Offline password cracking | Practitioner | Completed | Password storage | Relate password-hash properties and password strength to offline risk. |
| 11 | Password reset poisoning via middleware | Practitioner | Completed | Password reset | Understand risks in reset-link construction using untrusted request data. |
| 12 | Password brute-force via password change | Practitioner | Completed | Password change | Review verification and throttling on password-change flows. |
| 13 | Broken brute-force protection, multiple credentials per request | Expert | Completed | Brute-force defenses | Understand why controls must apply to the logical authentication attempt. |
| 14 | 2FA bypass using a brute-force attack | Expert | Postponed | MFA; Burp Suite | Build advanced Intruder Macro and session-handling skills in future training. |

## Progress summary

| Difficulty | Completed | Postponed |
|---|---:|---:|
| Apprentice | 3 | 0 |
| Practitioner | 9 | 0 |
| Expert | 1 | 1 |
| **Total** | **13** | **1** |

The final Expert lab is intentionally postponed until the Practical Pentesting stage, when advanced Burp Suite Intruder Macros and session handling will be studied. It is not marked completed or failed.

## Skills Developed

The completed labs provided practical exposure to comparing authentication responses, recognizing enumeration side channels, examining rate-limit and lockout assumptions, reviewing MFA and password-reset logic, understanding password-storage risk, and assessing persistent authentication. These experiences are a foundation for continued study, not a claim of comprehensive mastery.

## Future Training

Study advanced Burp Suite Intruder configuration, especially Macros, session-handling rules, and dynamic token maintenance. Then revisit “2FA bypass using a brute-force attack” only in the authorized PortSwigger lab environment.

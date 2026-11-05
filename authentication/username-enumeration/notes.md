# Username Enumeration

Username enumeration occurs when an application reveals whether an identifier corresponds to an account. It can reduce uncertainty for a later targeted attack, but the presence of a response difference does not by itself establish a vulnerability or meaningful impact.

## Why it happens

Applications may take different code paths for unknown usernames and known users with incorrect credentials. The difference can leak through text, status, response structure, redirects, processing time, or account lockout behavior. A recovery form may also disclose account existence if it reports different outcomes for registered and unregistered addresses.

## Observable signals

| Signal | Example of what to compare | Important caveat |
|---|---|---|
| Explicit message | “Unknown account” versus “Incorrect password” | Clear disclosure, but assess exposure and context. |
| Subtle content difference | Different wording, markup, field, or hidden value | Normalize dynamic content before drawing conclusions. |
| HTTP status or redirect | Different status, destination, or challenge page | Other policy or risk checks may explain it. |
| Response length | Stable byte or rendered-content difference | Templates, localization, and dynamic values can vary. |
| Timing | Repeatable processing-time difference | Network noise and server load create false positives. |
| Account lockout | A response or behavior changes for existing identities | Lockouts can also harm users; do not test against real accounts. |

### Categories

- **Explicit enumeration:** The application directly states whether the username exists.
- **Side-channel enumeration:** Account validity is inferred from an indirect observable property, such as status, response structure, or lockout behavior.
- **Timing-based enumeration:** A repeatable processing-time difference correlates with account validity. Treat single measurements as weak evidence.
- **Enumeration through account lockout:** Different lockout or retry behavior reveals whether an account is present or has reached a threshold.

These categories can overlap. “Subtly different responses” are a form of observable side channel; the label describes the signal, not necessarily a separate root cause.

## Controlled investigation

In an authorized lab or with accounts created for testing:

1. Capture a baseline request and response for each relevant authentication or recovery flow.
2. Change only the account identifier while keeping other conditions consistent.
3. Compare message, status, redirects, response length, and relevant response fields.
4. For timing, repeat comparable requests under similar conditions; compare distributions rather than relying on one request.
5. Check whether cookies, risk challenges, rate limits, or session state explain the difference.
6. Document reproducibility and uncertainty. Stop if testing could lock out or affect another person's account.

Burp Proxy history and Comparer can help inspect captured responses. Repeater is useful for a small number of controlled comparisons. Keep sensitive values redacted in notes. The broader [discovery workflow](../methodology/discovery-workflow.md) describes how to document evidence and root cause.

## Risk and defenses

Detailed messages help legitimate users diagnose sign-in errors, but can reveal which identifiers are registered and make targeted attacks more efficient. The actual risk depends on how sensitive account existence is, whether identifiers are public, and what other controls are present.

Defensive measures include:

- Use consistent externally visible messages and response structure for account-related failures where practical.
- Avoid status codes, redirects, or recovery outcomes that distinguish account existence without a deliberate product reason.
- Reduce timing differences where feasible; avoid claiming perfect timing uniformity.
- Apply layered, abuse-resistant rate limits and monitoring without allowing attackers to trivially deny service through lockouts.
- Provide safe recovery UX and notify users about suspicious activity without exposing account status to unauthenticated callers.
- Test the complete workflow, not only the login error string.

See [brute-force defenses](../password-attacks/brute-force.md) for rate limiting and lockout tradeoffs.

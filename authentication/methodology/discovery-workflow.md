# Authentication Vulnerability Discovery Workflow

Use this repeatable workflow in PortSwigger labs, local applications, or other explicitly authorized environments. The goal is to establish a defensible root cause, not to apply a memorized attack sequence. Keep requests within the approved scope and avoid testing real users' accounts.

## 1. Reconnaissance and endpoint mapping

Identify the application's authentication surface from its UI, documentation, and in-scope HTTP traffic. Map registration, login, logout, password change, password reset, MFA enrollment, MFA verification, recovery, and any “remember me” flow. Record methods, parameters, redirects, cookies, and relevant response headers.

Do not assume similarly named endpoints share the same controls. Trace each flow far enough to understand which server-side state it reads or changes.

## 2. Establish identity and session states

Describe the states visible to the application, such as anonymous, primary-credential-verified, MFA-pending, fully authenticated, and logged out. Determine which actions are available in each state and which cookie or server-side record represents it.

Map transitions, including failure, retry, cancellation, logout, password change, and recovery. Confirm whether successful primary authentication rotates the session identifier and whether MFA completion creates or upgrades the authenticated state.

## 3. Form a testable expectation

For each endpoint, write down the expected behavior before comparing responses. Use the same authorized test context and vary one factor at a time. Compare valid and invalid inputs without using other people's identities or credentials.

## 4. Investigate response differences

For login and recovery flows, compare messages, status codes, redirects, response lengths, relevant fields, and timing. Repeat timing comparisons under similar conditions and account for network and server variability. A difference is a lead, not proof: confirm that it is stable, attributable to account validity, and meaningful to an attacker under the stated threat model.

Assess lockout and throttling behavior with a small, approved test set. Do not create excessive traffic, lock out real accounts, or bypass a production control. Record whether limits are account-, source-, device-, or workflow-based and whether they apply consistently.

## 5. Review password and recovery flows

Trace password change and reset from request through server-side identity checks to completion. Verify that the current authenticated identity—not a caller-controlled identifier—determines which account is changed. For resets, inspect token binding, entropy, lifetime, single-use behavior, invalidation, and how the reset destination is constructed.

## 6. Review MFA and session behavior

Follow enrollment, challenge, verification, recovery, and post-verification transitions. Check that each step is bound to the same user and transaction, that incomplete verification does not grant full privileges, and that retries are appropriately limited. Review cookies and session lifecycle, including rotation after authentication and invalidation on logout or sensitive credential changes.

## 7. Use Burp Suite deliberately

Use Proxy history to understand a flow and Repeater for a small number of controlled comparisons. Intruder may help with bounded tests in a lab when specifically needed; configure scope and a conservative request rate, and stop if behavior risks affecting other users or service availability. Advanced session automation is covered in the [Macros and session handling reference](../burp-suite/macros-and-session-handling.md).

## 8. Confirm root cause and impact

Separate a symptom from its cause. Confirm the relevant trust assumption, identity binding, state transition, or missing control. Assess only demonstrated impact: which account state or action is affected, what preconditions exist, and what defenses limit it. Do not infer account takeover from an unexplained response difference alone.

## 9. Document and recommend a fix

For every investigation, record:

| Field | What to capture |
|---|---|
| Endpoint | In-scope route and feature. |
| HTTP method | Request method used. |
| Parameters | Relevant fields, headers, and cookies; redact secrets. |
| Authentication state | Anonymous, partially authenticated, fully authenticated, or other observed state. |
| Session state | Relevant session continuity, rotation, or invalidation behavior. |
| Expected behavior | Security property and expected response/state transition. |
| Actual behavior | Repeatable observation, with sanitized evidence. |
| Security assumption | Identity, trust boundary, or control the design relies on. |
| Observed weakness | The specific behavior that violates that assumption. |
| Root cause | The implementation or design error supported by evidence. |
| Impact | Demonstrated consequence and necessary preconditions. |
| Mitigation | Server-side change that addresses the root cause. |

Prefer mitigations that enforce identity and authorization server-side, normalize externally visible authentication behavior where appropriate, apply layered throttling, and make state transitions explicit. Re-test the fixed behavior rather than relying solely on the change description.

## Reusable checklist

- [ ] Confirm authorization, scope, and safe test accounts.
- [ ] Map login, registration, logout, password change/reset, MFA, and recovery endpoints.
- [ ] Record request methods, parameters, responses, cookies, and redirects.
- [ ] Identify authentication states and session transitions.
- [ ] Compare controlled valid/invalid cases and repeat observations.
- [ ] Review enumeration, throttling, lockout, and retry behavior without disrupting accounts.
- [ ] Check password-reset token binding and reset-link construction.
- [ ] Check MFA identity binding and incomplete-verification behavior.
- [ ] Check session rotation, cookie attributes, expiration, and invalidation.
- [ ] Confirm root cause and evidence before assessing impact.
- [ ] Recommend and verify a server-side mitigation.

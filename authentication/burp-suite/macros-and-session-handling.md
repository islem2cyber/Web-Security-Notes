# Burp Macros and Session Handling

**Learning reference for future practical training.** This material supports the intentionally postponed Expert lab, “2FA bypass using a brute-force attack.” It does not mean these features have been mastered. Practice only in an authorized PortSwigger lab or an explicitly approved test environment.

## What a Macro is

A Burp Macro is a recorded sequence of requests that can be replayed as a unit. A session-handling rule can invoke a Macro to obtain or refresh state needed for another request. A Macro can, for example, follow a legitimate test-account flow and capture current session or anti-CSRF values.

A Macro is distinct from ordinary Intruder payload configuration:

- Intruder payload configuration defines which values are substituted at marked positions.
- A Macro performs prerequisite requests or refreshes state.
- Session-handling rules determine when the Macro runs and how its result is applied.

Macros do not automatically understand the application's security model. They must be configured and verified against the actual workflow.

## State and token maintenance

Authentication workflows may use cookies, hidden form values, CSRF tokens, challenge identifiers, or other per-request values. A reliable test setup must preserve the correct identity and sequence:

1. Establish a test account's intended starting state.
2. Capture the normal sequence in Proxy history.
3. Identify which values are static, session-scoped, or refreshed per request.
4. Configure the Macro to replay only the required requests and extract the necessary values using the application's actual response structure.
5. Configure the session-handling rule to update the relevant cookie or parameter in the request being tested.
6. Send one request and verify that it remains associated with the expected test identity and challenge.

Names and UI details can change between Burp versions. Use the installed version's interface and documentation rather than assuming a specific menu path.

## Using state with Intruder

Before any bounded Intruder run, confirm that the Macro can obtain fresh state and that each request receives the correct session and dynamic values. Understand whether the application expects a new challenge, a stable session, or another transition for each attempt. Configure only the minimum required request positions and a small authorized test set. Do not disable protections or broaden scope to compensate for a broken Macro.

## Common configuration mistakes

- Recording a Macro while logged out, in the wrong account, or in a stale challenge state.
- Failing to extract a rotating cookie or CSRF token from the correct response.
- Updating only one token location when the request uses multiple related values.
- Reusing a one-time challenge or token after it has expired or been consumed.
- Running the Macro too frequently or at the wrong point in the request sequence.
- Treating a successful HTTP response as proof that the intended authenticated state was retained.
- Allowing an Intruder run to continue after the test session becomes invalid.

## Troubleshooting and verification

When a Macro fails, pause automation and compare a manual successful workflow with the recorded requests. Check the sequence, redirects, cookie jar, extracted values, token freshness, and whether the application's state changed between attempts. Use Proxy history and Repeater for a single controlled request at a time.

Verify session continuity by confirming, in the test environment, that:

- The expected test identity remains active.
- The server accepts the current session and token values.
- The request reaches the intended workflow state rather than a login page or expired challenge.
- The test has not crossed into an unintended account or privilege state.
- Logout or reset returns the session to the expected unauthenticated state.

Stop if identity or state is ambiguous. See the [Intruder overview](./intruder.md) and [Burp Suite notes](./notes.md).

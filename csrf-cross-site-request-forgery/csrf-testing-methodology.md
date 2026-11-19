# CSRF Testing Methodology

Use this workflow only in explicitly authorized labs or test environments with test accounts and agreed request limits.

## 1. Identify state-changing endpoints

Review UI actions, API routes, forms, and workflow transitions. Record the endpoint, method, expected request body, and resulting server-side state.

## 2. Identify authenticated requests

Determine which cookies or authorization credentials apply and which account state is required. Verify the operation requires the victim account to be logged in rather than merely relying on public or anonymous access.

## 3. Inspect cookie behavior

Record the cookie name, Domain, Path, HttpOnly, Secure, and SameSite attributes. Map whether the cookie is sent in same-origin, same-site, cross-origin, and cross-site contexts.

## 4. Check CSRF token presence

Identify whether a form field, request header, double-submit cookie, or framework mechanism supplies a token. Verify whether the token is rendered, regenerated, and consumed according to server behavior.

## 5. Examine token validation and session binding

Test missing, stale, malformed, mismatched, and unrelated-session tokens only within the approved scope. Determine whether the server validates against trusted session state and whether a token issued in one session is accepted in another.

## 6. Test request method variations

Compare supported methods and method override mechanisms. Verify that CSRF controls and state-changing behavior apply to the logical operation rather than only to one representation or method.

## 7. Examine SameSite cookie attributes

Use a current browser and controlled lab setup to observe the relevant request context. Distinguish same-origin, same-site, cross-origin, and cross-site relationships, and avoid relying on one browser as proof of all client behavior.

## 8. Inspect Origin and Referer validation

Observe whether the server checks Origin or Referer. Test missing headers and mismatched values only where this is within scope. Verify that weak substring or suffix checks are not being used.

## 9. Test relevant browser behavior

Build a minimal authorized proof of browser request behavior when appropriate. Confirm whether relevant cookies are attached and whether the application changes state. Do not submit external real accounts or destructive mutations.

## 10. Document impact and remediation

Record:

- Account and endpoint under test.
- Cookie and token behavior.
- Request context and browser behavior.
- Demonstrated state change.
- Root cause.
- Business impact.
- Remediation and regression tests.

## Reusable checklist

- [ ] Authorized test scope and accounts confirmed.
- [ ] State-changing endpoints mapped.
- [ ] Authentication and cookie behavior recorded.
- [ ] CSRF token generation and binding examined.
- [ ] Missing and mismatched token behavior checked.
- [ ] Request methods and overrides assessed.
- [ ] SameSite and Secure attributes documented.
- [ ] Origin and Referer behavior checked.
- [ ] Browser result confirmed without destructive activity.
- [ ] Impact, root cause, and remediation documented.

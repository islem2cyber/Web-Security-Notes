# Access Control Discovery Workflow

Use this workflow to investigate authorization behavior in explicitly authorized web applications and APIs. The goal is to establish whether a user can access resources or actions outside the intended authorization boundary, and to confirm the server-side trust assumptions behind the behavior.

## 1. Map the authorization surface

Enumerate the primary application features and identify the resource boundaries. Review login, logout, account settings, admin panels, user dashboards, document management, profile editing, payment or order management, and any API endpoints that act on object identifiers or privileged operations.

For each route, record:

- HTTP method.
- Route or endpoint pattern.
- Authentication state.
- Role or privilege data available from the UI.
- Any request parameters or headers influencing authorization.

Do not assume similarly named routes share the same controls. Administrative functionality may be hidden, repurposed, or exposed in unexpected ways.

## 2. Establish the expected authorization model

Before testing, define the expected policy in plain language.

Examples:

- Standard users may view only their own invoices.
- Admins may create new users and assign roles.
- Users may update only their own profile fields.
- MFA verification must be bound to the same user and request context.

A useful test writes down the intended rule, then observes whether the application behaves consistently under controlled variations.

## 3. Build a controlled authorization matrix

Use only accounts and resources explicitly provided or created for the assessment. Map the expected permissions for each test identity (for example, anonymous, standard user A, standard user B, and administrator) against relevant actions such as read, update, delete, and approve. Record the policy expectation before sending test requests.

## 4. Test identity and session boundaries

Compare what different authenticated identities can access. Confirm that session state and identity binding are correct. Assess whether the application uses trusted server-side session data or accepts client-supplied role or ownership values.

Look for:

- Access to admin functions by normal users.
- User A viewing or editing user B's records.
- User-controlled IDs that produce different resource responses.
- Inconsistent behavior between UI and API endpoints.

## 5. Check direct object references

Review URLs, query strings, JSON bodies, headers, and cookies for object identifiers and privilege-related parameters. Attempt small controlled changes to the identifier while keeping the rest of the request constant. Assess whether the server checks ownership, object existence, and policy before returning or mutating the resource.

Document whether the issue is:

- Read-based.
- Write-based.
- Delete-based.
- Function-level.
- Horizontal, vertical, or context-dependent.

## 6. Review parameter tampering and hidden values

Inspect hidden fields, cookies, and custom headers. Confirm whether the application trusts values such as `role`, `isAdmin`, `uid`, `accountId`, or `privilege` supplied by the client.

A server should derive authorization from trusted server-side state. If user-controlled values change the authorization decision without verification, the application is likely vulnerable to parameter-based access control.

## 7. Test workflow and multi-step transitions

Check whether authorization only applies at the initial form page or first step, but not at the final operation. Attackers may skip a validation step by submitting a crafted request at a later stage.

Examples include:

- Directly posting a final `approve` request without going through the prerequisite workflow.
- Updating a resource after a step that should have required a different role or state.
- Reusing a completed token or stale workflow state to access sensitive operations.

## 8. Investigate route and method inconsistencies

Check for mismatches between route matching, HTTP method use, and authorization enforcement. Review documented or observed alternate routes and methods, case handling, suffix rules, or proxy rewriting. Do not assume a particular header or path variant has an effect without verifying the application and proxy configuration behavior.

Things to review:

- `GET` vs `POST` access to the same action.
- Case differences or path normalization issues.
- `X-Original-URL` or similar forwarded values.
- Front-end-only checks that are bypassed by direct API requests.

## 9. Assess impact and root cause

For every observation, confirm:

- The identity under test.
- The resource or action affected.
- The sensitive permission boundary.
- Whether the check is enforced server-side.
- Whether the issue is a misconfiguration, missing check, or flawed trust decision.

Impact is not determined by a guessed URL alone. It is determined by the demonstrated access and the application policy the server is supposed to enforce.

## 10. Document mitigation

Prefer fixes that are straightforward and enforceable:

- Require server-side authorization for each privileged action.
- Base decisions on trusted session state and role checks, not client-supplied values.
- Use centralized permission logic and resource ownership checks.
- Validate workflow state and apply consistent checks at every sensitive step.
- Reject unexpected methods, incompatible routes, stale tokens, and tampered request fields.

## Reusable checklist

- [ ] Define the expected policy for each resource and action.
- [ ] Use only in-scope test identities and map their role boundaries.
- [ ] Compare the same action across identities and record the expected and observed outcomes.
- [ ] Check direct object references and ownership checks.
- [ ] Review hidden values, cookies, and custom headers.
- [ ] Assess multi-step workflows and sensitive transitions.
- [ ] Verify route, method, and proxy behavior before concluding that a variant bypasses a control.
- [ ] Determine if the authorization decision is server-side only.
- [ ] Record the demonstrated impact and supporting evidence.
- [ ] Recommend a fix that enforces allowlists and least privilege.

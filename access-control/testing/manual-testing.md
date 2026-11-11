# Manual Authorization Testing

This note provides a practical, evidence-based approach to testing access control in explicitly authorized environments. The objective is not to guess randomly, but to verify that authorization is enforced consistently and that state, ownership, and workflow conditions are respected.

## 1. Establish the policy

Before testing, write down the intended rule for each route and action. This can be as simple as:

- Users can only view their own records.
- Admins may access the user management panel.
- Managers can approve only tickets assigned to their department.

A test is meaningful only when it can be compared to the intended behavior.

## 2. Create a controlled test matrix

Use only identities and resources within the authorized scope. Record the expected permissions for each test account and action before testing. For example, compare two user-owned test accounts and an administrator against read, update, and delete operations on test records. Do not substitute real users' identifiers or data.

## 3. Compare identity states

Test the same request with different authenticated accounts. Record whether the route or action behaves consistently with the expected policy. A route that shows different data for different users is a lead to investigate, not proof by itself.

## 4. Review object references

In the authorized test setup, vary one object identifier or authorization-relevant parameter at a time while keeping the rest of the request constant. Use a known test resource and confirm whether the server permits an operation that the documented policy denies. A changed response alone is not proof of unauthorized access.

## 5. Focus on sensitive steps

Check not only the UI entry points but also the final API actions behind them. Attackers often bypass front-end protection by posting directly to a later-stage endpoint.

## 6. Verify workflow state

Check whether the application validates the state at every sensitive step. Many workflow issues arise when the first step is checked but the final mutation is not.

## 7. Record evidence and impact

Document:

- The route.
- Request method.
- User identities involved.
- The object or action targeted.
- The expected policy.
- The observed outcome.
- Whether the issue is read-, write-, or function-level.
- The test identity and resource ownership used, without recording unnecessary personal data.
- The root cause and potential impact.

For state-changing operations, use disposable test data and obtain explicit approval for the operation. Avoid destructive actions against shared or production data. Stop and report if a request affects an unintended account, resource, or service.

## Safe assessment reminder

Only assess access control in an explicitly authorized environment. Use a small, controlled set of accounts and requests. Do not test against real users or real data outside the agreed scope. Stop if behavior demonstrates a service-impacting issue or affects legitimate users.

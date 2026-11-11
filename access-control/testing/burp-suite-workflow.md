# Burp Suite Workflow for Access Control Testing

Burp Suite is useful for mapping routes, replaying requests, modifying parameters, and comparing authenticated behaviors. Use it in an explicitly authorized environment and keep investigations bounded.

## Recommended workflow

1. Capture the application flow with Proxy.
2. Identify the relevant route, method, parameters, cookies, and headers.
3. Compare a normal-user request against the intended admin or owner path.
4. Replay or modify the object identifier, endpoint, or parameter.
5. Confirm whether the server enforces the correct policy.
6. Document the exact request and response evidence.

## Useful Burp features

- Proxy: capture and compare requests.
- Repeater: test object IDs and alternate parameters.
- Intruder: assess parameter variation under a controlled, bounded set.
- Sequencer: only where relevant to token integrity or session-state checks.
- Collaborator or logging tools: helpful for validating out-of-band behavior when ethically approved.

## Practical guidance

- Compare requests from distinct authenticated accounts.
- Check for differences between UI-generated requests and direct API requests.
- Review hidden fields and cookies for privilege-related values.
- Confirm the backend decision rather than trusting the HTML/JS surface.

## Example pattern

1. Log in as a standard user.
2. Capture a request to view a resource with an ID.
3. Modify the ID to another user's value.
4. Replay the request.
5. Observe whether the server authorizes or rejects it.
6. Confirm whether the issue is user scope, object ownership, or role enforcement.

## Important reminder

Burp Suite is a tool, not a security decision. The real judgment comes from whether the server-side policy enforces the intended access boundaries and whether the observed behavior is consistent with the documented authorization model.

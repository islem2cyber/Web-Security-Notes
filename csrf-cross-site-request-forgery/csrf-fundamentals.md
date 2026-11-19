# CSRF Fundamentals

Cross-Site Request Forgery (CSRF) causes a user's browser to perform an unwanted state-changing action in a web application where the user is authenticated. The application receives a request that appears to belong to the user, even though it was initiated by another origin.

## Browser cookie behavior

When a browser sends a request, it may attach relevant cookies according to cookie domain, path, SameSite policy, secure constraints, and browser behavior. Authentication cookies may therefore be sent to a legitimate application origin even when a request was initiated from an attacker-controlled page.

Automatic cookie attachment is not consent or authorization. It explains why the server can receive a valid session cookie for an action it did not intend the user to authorize.

## Authentication versus authorization

- Authentication establishes or validates identity, for example through a login session.
- Authorization decides whether that identity may perform an action or access a resource.
- CSRF usually abuses an authenticated session; it does not prove that the victim is authorized to perform the action.

Authorization is also needed for CSRF defenses: whether an operation is legitimate depends on the user's explicit intent and server-side session context.

## Required conditions

A practical CSRF scenario generally requires:

1. An authenticated user and a state-changing endpoint.
2. Request parameters the attacker can predict or supply.
3. No effective requirement for user intent or server-side request uniqueness.
4. Browser rules that allow the request and relevant credentials to reach the target.

The exact browser context and method determine whether the request is viable.

## CSRF and SOP

The Same-Origin Policy prevents many script behaviors, such as reading a response across origins. It does not generally prevent a cross-origin page from causing the browser to submit a simple HTML form or load a resource.

The browser is acting as a messenger, not exposing a readable response. SOP and credential policies restrict some interactions but do not prove that a mutation request was authorized.

## CSRF and XSS

CSRF makes a browser send an unwanted request. XSS causes attacker-controlled script to execute in the application's origin.

An XSS flaw is often more powerful because script can interact with same-origin pages and may read or manipulate CSRF tokens. CSRF usually relies on browser request behavior and is mitigated by request-origin and anti-CSRF controls.

## GET and POST

GET requests should not change application state. Even then, GET requests can trigger unintended side effects if a server violates HTTP semantics or if browser navigation is involved. POST requests are the most common CSRF target, but CSRF is fundamentally about state-changing intent rather than method alone.

Use the server's HTTP semantics as the source of truth and reject state changes through unsafe or unexpected methods.

## State-changing requests

Examples include changing an email address, updating profile privileges, creating a transfer, changing password, deleting data, or approving a workflow step.

CSRF testing should identify which endpoint actually changes state and whether authentication alone is used to authorize that change.

# CSRF Fundamentals

Cross-Site Request Forgery (CSRF) causes a user's browser to perform an unwanted state-changing action in a web application where the user is authenticated. The application receives a request that appears to belong to the user, even though it was initiated by another origin.

## Browser cookie behavior

When a browser sends a request, it may attach relevant cookies according to cookie domain, path, SameSite policy, secure constraints, and browser behavior. These cookies are ambient credentials: the browser can attach them to a request to the application even when the request was initiated from an attacker-controlled page.

The key issue is that the browser attaches ambient credentials to a request that the server does not adequately verify as an intended application request. Automatic cookie attachment is not user consent.

## Authentication, authorization, and CSRF protection

- **Authentication** establishes or validates an identity, often represented by a session cookie.
- **Authorization** determines whether that identity may access a resource or perform an action. It enforces permissions; it does not prove that the user initiated or intended a particular request.
- **CSRF protection** helps the server distinguish requests arising from the application's legitimate interaction flow from requests an attacker induces a browser to send. A token or origin check is evidence about request context, not proof of human intent.

These controls address different questions. A sensitive action may need both an authorization check and CSRF protection. For high-risk actions, applications may additionally require explicit confirmation or reauthentication.

## Conditions for CSRF

A practical CSRF scenario generally requires:

1. A victim has credentials for an application, commonly a session cookie, that the browser may attach automatically.
2. The attacker can cause the victim's browser to send a request to a state-changing endpoint while those credentials are attached.
3. The attacker can supply or predict any request data needed for the action, or the endpoint does not require attacker-unknown data.
4. The server does not adequately verify the request's legitimacy using effective CSRF controls.

The exact browser context, credential type, request method, endpoint, and defenses determine whether a scenario is viable. A unique request value is not a universal prerequisite for a vulnerability; it matters only when the application requires such a value and whether it is protected against attacker use.

## CSRF and SOP

The Same-Origin Policy prevents many script behaviors, such as reading a response across origins. It does not generally prevent a cross-origin page from causing the browser to submit a simple HTML form or load a resource.

The browser can act as a messenger without exposing a readable response. SOP and credential policies restrict some interactions but do not establish that a state-changing request was intended by the user.

## CSRF and XSS

CSRF makes a browser send an unwanted request. XSS causes attacker-controlled script to execute in the application's origin.

An XSS flaw is often more powerful because script can interact with same-origin pages and may read or manipulate CSRF tokens. CSRF usually relies on browser request behavior and is mitigated by request-origin and anti-CSRF controls.

## GET and POST

GET requests should not change application state. Even then, GET requests can trigger unintended side effects if a server violates HTTP semantics or if browser navigation is involved. POST requests are the most common CSRF target, but CSRF is fundamentally about state-changing intent rather than method alone.

Use the server's HTTP semantics as the source of truth and reject state changes through unsafe or unexpected methods.

## State-changing requests

Examples include changing an email address, updating profile privileges, creating a transfer, changing password, deleting data, or approving a workflow step.

CSRF testing should identify which endpoint actually changes state and whether authentication alone is used to authorize that change.

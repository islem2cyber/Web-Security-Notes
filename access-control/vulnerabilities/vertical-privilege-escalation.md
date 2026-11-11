# Vertical Privilege Escalation

## Definition

Vertical privilege escalation is when a user accesses functionality or data normally reserved for a higher-privilege role, such as an administrator or support user. This is a common broken-access-control flaw and often results from a missing or weak authorization check.

## Typical scenarios

- A normal user calls an administrative endpoint.
- A standard user modifies an order they should not be able to access.
- A customer reaches an internal support or finance panel.
- A low-privilege account uses a hidden or guessed URL to reach an admin function.

## Missing authorization checks

The most direct form of vertical escalation occurs when the server fails to verify whether the current user has the required role for a restricted endpoint.

Example:

```http
GET /admin/users HTTP/1.1
Host: example.com
Cookie: session=VALID_SESSION
```

If the server accepts this request without checking the authenticated user's role, the application is vulnerable even if the page was not linked in the UI.

## Unprotected administrative functionality

Sometimes the application exposes administrative controls in a way that is obvious to an attacker. This may include:

- A hidden admin panel route.
- A route that is not rendered in navigation.
- A direct path discovered from source code, JS, or a leaked comment.
- An API route that accepts privileged actions without a role check.

Hidden or non-obvious URLs do not provide security. Security must come from enforced authorization, not URL secrecy.

## Security by obscurity is not sufficient

The idea that an endpoint is “safe because it is hidden” is unreliable. Attackers routinely discover admin paths via:

- Response bodies and JavaScript.
- Source maps and minified bundles.
- Robots.txt and sitemap-like files.
- Wordlists and endpoint fuzzing.
- Error messages and references in forms.

A route is not protected by being difficult to find.

## Administrative endpoint discovery

The first step in evaluating vertical escalation is identifying the privileged surface. Review:

- Admin dashboards and navigation.
- User settings pages.
- Backup, export, and configuration routes.
- API endpoints with names like `admin`, `manage`, `delete`, `approve`, `export`, or `settings`.
- Interaction flows that should be gated by privileges.

## Server-side authorization requirement

The server must derive the caller's permissions from trusted server-side state and compare them to the requested privilege boundary before allowing access. A route that contains “admin” in the path is not enough. Authorization must be enforced on every relevant request.

## Safe request examples

The following examples are illustrative and should be used only in explicitly authorized testing environments.

```http
GET /admin/users?id=42 HTTP/1.1
Host: example.com
Cookie: session=low_privilege_user_session
```

```http
POST /admin/disable-user HTTP/1.1
Host: example.com
Cookie: session=low_privilege_user_session
Content-Type: application/x-www-form-urlencoded

userId=42
```

A properly enforced server will reject these requests unless the authenticated identity has the required admin role and the operation is valid for that context.

## Impact

Vertical escalation can expose:

- User management functions.
- Subscription and payment changes.
- Sensitive views or configuration data.
- Ability to modify or delete records across the platform.

The impact depends on the privilege level gained and the application's trust boundaries.

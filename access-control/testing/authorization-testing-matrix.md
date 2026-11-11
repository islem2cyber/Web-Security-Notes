# Authorization Testing Matrix

This matrix helps structure authorization testing in a controlled, repeatable way.

| Area | What to test | Typical check | Risk |
|---|---|---|---|
| Role boundary | Admin-only routes | Access from normal user | Vertical escalation |
| Ownership | Object-level IDs | Access another user's record | Horizontal escalation |
| Workflow | Final step logic | Skip earlier checks | Multi-step bypass |
| Parameter trust | Client role values | Change `role` or `isAdmin` | Parameter tampering |
| Routing | Alternate paths | Use alternate casing or method | URL mismatch |
| Direct object access | ID manipulation | Modify object ID in request | IDOR |
| Function access | Privileged actions | Call `approve`, `export`, `delete` directly | BFLA |
| Headers and proxies | `Referer`, `X-Original-URL` | Trust forwarded metadata | Platform misconfiguration |

## Example test matrix

| Route | User A | User B | Admin | Expected result | Observed result |
|---|---|---|---|---|---|
| `/profile?id=77` | Yes | No | Yes | User A sees only own profile | User A sees another user's profile |
| `/admin/users` | No | No | Yes | Access denied for non-admin | Access granted |
| `/api/export` | Yes | No | Yes | Export restricted to admin | Non-admin export succeeds |
| `/order/123/approve` | No | No | Yes | Only admin can approve | Normal user approves order |

Use this structure as a checklist for consistent evidence gathering and documented root cause analysis.

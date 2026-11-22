# File path traversal, validation of start of path

- **Difficulty:** Practitioner
- **Status:** SOLVED

## Vulnerability concept

The application validates the beginning of a path rather than the complete final location.

## Root cause

A string-prefix or start-of-path check does not represent a directory containment boundary.

## Technical explanation

Input beginning with an approved directory string can later contain traversal components or can refer to text outside a real directory boundary. A canonical path check resolves the complete location and asks whether it remains within the trusted base.

No exact lab payload or response is claimed here because those details were not supplied.

## What the lab teaches

- `startsWith("trusted directory")` is not a directory boundary check.
- Prefix text can be followed by `..`, alternate separators, or sibling-directory text.
- The final resolved path must be checked after normalization.

## Security implications

A prefix accepted for one trusted-looking location can enable access to another location with the application's filesystem privileges.

## Proper remediation

- Resolve the final candidate and trusted base canonically.
- Check containment using filesystem-aware APIs.
- Reject absolute paths if only relative names are expected.
- Prefer an allowlist or identifier mapping.

## Key takeaways

Validation must use path semantics and the complete resolved location.

## Personal observations

To be added.

# File path traversal, simple case

- **Difficulty:** Apprentice
- **Status:** SOLVED

## Vulnerability concept

The application uses a user-controlled path value to retrieve a file without sufficient protection against directory traversal.

## Root cause

The file-access operation treats request data as a filesystem location or combines it unsafely with a base directory.

## Technical explanation

In a vulnerable design, traversal components in a user-controlled value can cause the resolved path to leave the intended file directory. The exact application behavior, parameter, and payload are not supplied here, so no lab-specific payload or response is claimed.

## What the lab teaches

- A filename parameter can become a filesystem trust boundary.
- Relative path components can alter the resolved location.
- Successful file access requires both traversal capability and application filesystem permissions.

## Security implications

An unauthorized file read may expose configuration, source, credentials, or operating-system information. Actual impact depends on the application's implementation and process privileges.

## Proper remediation

- Prefer server-side identifiers mapped to allowlisted files.
- If paths are required, resolve against a fixed base directory.
- Validate canonical containment before file access.
- Apply least privilege to the application filesystem account.

## Key takeaways

The user must not be able to choose an arbitrary filesystem location merely by controlling a path component.

## Personal observations

To be added.

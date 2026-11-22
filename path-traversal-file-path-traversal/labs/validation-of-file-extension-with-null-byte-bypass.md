# File path traversal, validation of file extension with null byte bypass

- **Difficulty:** Practitioner
- **Status:** SOLVED

## Vulnerability concept

The application validates a file extension that may differ from the path interpreted by a legacy native API or string-conversion layer.

## Root cause

An extension check is treated as a security boundary, while a null byte or legacy string-termination behavior changes the effective path.

## Technical explanation

In legacy environments, a null byte represented by `%00` could terminate a C-style string and cause text after the terminator to be ignored by the native API. The application may validate the full text but the final file operation may use a truncated text form.

Modern runtimes often reject embedded nulls, so this lab result is implementation-specific and must not be generalized to all systems.

## What the lab teaches

- Extension validation does not guarantee safe file access.
- Legacy string termination can differ from application-string semantics.
- Modern language behavior should be confirmed for the target runtime.
- The final filesystem boundary is a stronger control.

## Security implications

A vulnerable implementation could reach an unintended file despite an extension or suffix check. Impact depends on the application's privileges and available files.

## Proper remediation

- Reject null bytes and control characters where unsupported.
- Replace extension checks with allowlists or identifier mappings.
- Resolve and validate canonical containment.
- Avoid unsafe native conversions and legacy APIs.

## Key takeaways

Null-byte behavior is historical and implementation-specific; it is evidence of a root-cause mismatch, not a modern universal bypass.

## Personal observations

To be added.

# Web shell upload via extension blacklist bypass

- **Difficulty:** Practitioner
- **Status:** SOLVED

## Vulnerability concept

A blacklist tries to prohibit known script extensions but misses an accepted server-side extension or case-specific interpretation.

## Root cause

The application uses a deny list and relies on filename suffix matching, while server mappings or platform behavior do not align with that list.

## Technical explanation

Blacklist matching is fragile. Extension aliases, multiple extensions, casing, server-specific mappings, normalization, and alternate representations can evade a deny list. The lab title supports this concept, but no exact payload or personal walkthrough is claimed.

See [File Validation](../file-validation.md).

## What the lab teaches

- A blacklist does not define every executable format.
- Server interpretation determines whether a suffix is active.
- Filename matching must account for normalization and platform behavior.

## Security implications

Dangerous server-side content may receive an accepted filename and execute under vulnerable configuration.

## Proper remediation

- Use a strict allowlist for required formats.
- Generate the final server-side filename and extension.
- Disable script interpretation in upload directories.
- Check format bytes and structure.
- Keep server mappings aligned and remove unnecessary interpreters.

## Key takeaways

An allowlist plus generated names is more reliable than attempting to deny every dangerous suffix.

## Personal observations

To be added.

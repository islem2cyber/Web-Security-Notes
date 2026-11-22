# File path traversal, traversal sequences blocked with absolute path bypass

- **Difficulty:** Practitioner
- **Status:** SOLVED

## Vulnerability concept

The application blocks traversal syntax but may accept an absolute path that points outside the intended directory.

## Root cause

The control checks traversal components or raw path text instead of validating the final resolved path and containment within a trusted base directory.

## Technical explanation

An absolute path can identify a file location without containing `..` components. If an application validates only whether traversal sequences exist and still passes absolute paths to a filesystem API, the validation model does not match filesystem semantics.

No exact payload or application response is claimed here because those details were not supplied.

## What the lab teaches

- Blocking `../` alone is not sufficient.
- Absolute and relative path forms can represent the same unsafe location.
- Final canonical containment is more reliable than sequence detection.

## Security implications

An attacker may read a file outside the intended directory if application permissions allow it.

## Proper remediation

- Reject absolute paths where relative filenames are expected.
- Map allowed names to server-controlled paths.
- Resolve against a fixed base and verify canonical containment.
- Do not rely on traversal-sequence filtering alone.

## Key takeaways

The bypass occurs when raw-string validation does not represent filesystem path semantics.

## Personal observations

To be added.

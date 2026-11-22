# Path Traversal Remediation

The most reliable remediation removes application control over filesystem paths instead of trying to recognize every traversal string.

## Avoid arbitrary filesystem paths

Do not accept a complete path from a user and pass it directly to `open`, `sendFile`, `Include`, or another filesystem API.

Prefer a server-side identifier:

```text
report_id = "quarterly-report"
```

Then map that identifier to a trusted path maintained by the application.

## Use server-side allowlists

Validate an identifier against an allowlist or database record. If a user is allowed to download a report, authorize and retrieve the server's known record before opening a mapped file.

An allowlist of legitimate paths or filenames is safer than a denylist of `../` and encoded variants.

## Normalize before validation

Where arbitrary path data is unavoidable, canonicalize or resolve it using the platform's filesystem API only after applying a consistent decoding policy. Validate the final canonical path against the intended base directory.

The following Python example is illustrative and assumes the application wants to read a file beneath a fixed base:

```python
from pathlib import Path

base = Path("/srv/app/files").resolve()
candidate = (base / user_value).resolve()

try:
    candidate.relative_to(base)
except ValueError:
    raise ValueError("path escapes the allowed base directory")

if not candidate.is_file():
    raise FileNotFoundError(candidate.name)

with candidate.open("rb") as file:
    data = file.read()
```

This example does not authorize access to `candidate`. It only establishes containment. It must be combined with authorization and a suitable file-size or streaming policy. Symlinks and platform behavior should be evaluated for the target runtime. `Path.resolve()` behavior can differ across Python versions and missing paths, so the application should be tested with its actual runtime.

## Validate the final boundary

A final containment check should account for:

- Relative and absolute paths.
- Separator semantics.
- Canonicalized names.
- Symlinks or directory junctions.
- Case sensitivity or insensitive filesystem behavior.
- The final file, not only the parent directory.

## Use safe filesystem APIs

Use APIs designed for the language and platform. Prefer opening a resolved, known file handle or using framework-provided safe download functions. Avoid shell commands that concatenate filenames into a command string.

## Apply least privilege

Run the application with the minimum OS permissions needed for intended file operations. Restrict readable directories, service accounts, containers, and mount points.

Least privilege reduces impact but does not correct unsafe path construction.

## Do not rely on incomplete checks

Avoid security boundaries based only on:

- A traversal-string denylist.
- One-pass string replacement.
- A string prefix that is not canonical.
- File-extension checks.
- URL decoding without knowing every processing stage.
- Null-byte filtering in modern runtimes.

## Add security tests

Add tests for:

- Normal files.
- Missing files.
- `.` and `..` components.
- Absolute paths.
- Nested or overlapping traversal strings.
- Encoded values when the application intentionally decodes them.
- Alternate platform separators.
- Symlinks or junctions.
- Identifiers that contain no file path.
- Attempts to escape the base directory.

Security tests should assert that the server rejects or safely maps the input and that no file outside the base is opened.

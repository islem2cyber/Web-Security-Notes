# Path Traversal Fundamentals

Path traversal occurs when an application uses attacker-influenced path data to access a location outside the filesystem area intended for that request.

## Terminology

"Directory traversal" and "file path traversal" describe the same broad class of file-access weakness. Directory traversal emphasizes movement between directories, while file path traversal emphasizes unsafe construction or validation of a file path.

An application is not inherently vulnerable merely because a request contains a filename parameter. The result depends on how the server combines that value with a filesystem base, validates it, and opens or sends the resolved file.

## Relative and absolute paths

A relative path is interpreted from a process working directory or an explicitly chosen base:

```text
var/app/images/avatar.jpg
```

An absolute path identifies a location from a filesystem root:

```text
/var/app/images/avatar.jpg
```

The Windows equivalent might use a drive prefix and backslashes:

```text
C:\app\images\avatar.jpg
```

## Basic file-path construction

A typical application may combine an uploaded or selected filename with a fixed directory:

```text
Base directory: /srv/application/images/
User value:     avatar.jpg
Final path:     /srv/application/images/avatar.jpg
```

If the filename is not validated or mapped safely, `../` can move upward:

```text
Base directory: /srv/application/images/
User value:     ../config/settings.ini
Resolved area:  /srv/application/config/settings.ini
```

This example illustrates path semantics; it is not a claim that a particular lab used this filename or produced this response.

## Platform-specific separators

The separator and path grammar differ across platforms. `/` is common on Unix-like systems, while `\` is common in Windows path syntax. Some APIs accept alternate separators or normalize them differently. URL separators and filesystem separators are related but not identical: URL data must be decoded before it is interpreted as a filesystem path only if the application actually defines that processing model.

Applications must not assume that a string containing no `/` is safe. The relevant runtime may recognize `\`, drive paths, device names, Unicode normalization, repeated separators, or platform-specific semantics.

## Why arbitrary filenames are dangerous

The filesystem is a trusted resource boundary. Accepting a user-supplied filename and using it directly for file access may allow:

- Reading of secrets or configuration files.
- Disclosure of source code or credentials.
- Access to application, operating-system, or backup files.
- Exposure of files that are not meant to be downloadable.
- Additional impact if the same mechanism supports writes or includes, because the consequence can extend beyond reading.

Actual impact depends on the application's permissions and on whether the affected path can be read or modified. A traversal string alone does not prove that sensitive data was reached.

## Core defense

Prefer an identifier or allowlisted name and map it to a server-controlled path. Where a filesystem path must be accepted, resolve it only after combining it with a fixed trusted base and verify the final canonical path remains inside that base.

Continue with [Path normalization](./path-normalization.md), [Traversal validation and filtering](./validation-and-filtering.md), and [Remediation](./remediation.md).

# Web shell upload via path traversal

- **Difficulty:** Practitioner
- **Status:** SOLVED

## Vulnerability concept

An upload filename influences the destination path, allowing directory traversal to place a file outside the intended upload directory.

## Root cause

The application uses user-controlled path components in filesystem operations instead of a fixed destination and server-generated filename.

## Technical explanation

A traversal sequence or alternate path representation may cause the final destination to move outside the upload directory. If the resulting location is executable and web-accessible, uploaded server-side content may create a web shell. Exact lab payloads and captured requests are not reproduced here.

See [Filename Security](../filename-security.md) and [Path Traversal](../../path-traversal-file-path-traversal/README.md).

## What the lab teaches

- Upload filenames can become filesystem paths.
- Path traversal and file upload vulnerabilities can combine.
- Canonicalization and final destination validation matter.

## Security implications

Files may be written outside the intended directory, overwrite data, or become executable in a sensitive location.

## Proper remediation

- Generate filenames on the server.
- Select a fixed upload base directory.
- Resolve and verify the final canonical path.
- Reject or remove path components.
- Prevent execution and overwrite.

## Key takeaways

The original filename must never determine the destination through an unsafe join or write operation.

## Personal observations

To be added.

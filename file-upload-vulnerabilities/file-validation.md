# File Validation

File validation must decide what is allowed before untrusted data is moved to durable storage or passed to a parser.

## Allowlist and blacklist

An allowlist accepts a small set of known-safe requirements. It is preferred to a blacklist, which tries to deny known-dangerous values. File formats contain many valid encodings and platform-specific transformations, so blacklist lists are incomplete.

Useful allowlist properties can include:

- a small set of approved extensions;
- a set of permitted MIME types;
- expected magic bytes or a canonical format;
- a maximum decoded and encoded size;
- safe dimensions or metadata;
- permitted archive entries;
- generated filenames instead of client names.

No individual property proves that a file is safe. Combine controls and verify the format with a parser that has been isolated and configured safely.

## Extension handling

An extension is a suffix convention, not proof of file format. Validation must account for:

- case sensitivity and platform behavior;
- multiple extensions;
- alternate or unexpected representations;
- server or framework-specific extension mappings;
- trailing characters or separators;
- normalized names and Unicode behavior;
- overwrite and collision handling.

Application code and the web server may interpret names differently. Normalize and validate the final server-controlled path rather than only the original HTTP field.

## Filename validation

The original filename is client-controlled. Reject or remove path components and control characters. Prefer a server-generated opaque filename and a fixed destination directory. A defense that removes `../` one time may fail if there are alternate encodings or later transformations.

## MIME and signature validation

The request Content-Type is declared metadata. A signature describes a recognized format in the file content. Neither is a complete content-security check. For example, data can have a valid-looking signature while containing parser-specific or embedded active content.

See [Content-Type and MIME](./content-type-and-mime.md) and [File Signatures and Magic Bytes](./file-signatures-and-magic-bytes.md).

## Size and resource limits

Enforce:

- request body limits;
- individual file limits;
- total upload limits;
- decompressed-size limits for archives;
- limits on item count and dimensions;
- processing timeouts;
- available disk and memory limits.

Limits protect availability even when type validation succeeds.

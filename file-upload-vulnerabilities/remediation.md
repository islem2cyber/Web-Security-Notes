# Remediation

## Secure upload architecture

A secure implementation should:

- require authentication and authorization;
- generate server-side filenames;
- use strict allowlists for approved formats;
- validate files on the server;
- combine extension, MIME, and signature checks where useful;
- verify format structure with a maintained parser;
- never trust client-supplied metadata;
- store uploads outside the web root when possible;
- disable execution in upload directories;
- run with least privilege;
- enforce file size and request limits;
- normalize and validate paths;
- use safe temporary storage;
- scan files when appropriate;
- process files securely and atomically;
- prevent or close race-condition windows;
- log and monitor upload activity.

## Practical secure workflow

```text
authorization
  -> enforce request and file-size limits
  -> parse multipart safely
  -> reject extra or unexpected fields
  -> validate server-side format and structure
  -> reject archive or embedded active content if unnecessary
  -> process in an isolated temporary location
  -> generate a random server filename
  -> atomically move to a non-executable destination
  -> store metadata separately
  -> return an opaque identifier
  -> authorize retrieval
  -> serve with controlled headers
```

Treat deletion and processing as lifecycle operations. A file that is valid after processing may need to be re-encoded or copied to a clean container before publication.

## Header and response controls

Set an explicit safe content type for user downloads, use `Content-Disposition: attachment` where appropriate, and consider `X-Content-Type-Options: nosniff`. Browser CSP and sandboxed download origins can provide defense in depth but do not replace upload validation.

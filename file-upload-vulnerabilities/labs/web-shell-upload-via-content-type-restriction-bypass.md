# Web shell upload via Content-Type restriction bypass

- **Difficulty:** Apprentice
- **Status:** SOLVED

## Vulnerability concept

The server validates a client-supplied MIME or multipart `Content-Type` rather than the actual file bytes or a derived server-side format.

## Root cause

Declared MIME metadata is trusted as an authorization or format decision. The application does not verify that the upload bytes match an approved format.

## Technical explanation

The `Content-Type` header is request metadata and can be supplied independently of the file content. This lab demonstrates why a MIME allowlist must not rely solely on client-supplied data. No exact lab payload or request capture is reproduced here.

See [Content-Type and MIME](../content-type-and-mime.md).

## What the lab teaches

- Browser and client MIME metadata is not authoritative.
- Server-side file validation must inspect the received data.
- Declared type, extension, and actual format are separate properties.

## Security implications

Server-side script or active content may pass a declared-type check and later be interpreted or rendered. Impact depends on storage and server configuration.

## Proper remediation

- Ignore or use client MIME values only as hints.
- Detect approved formats from server-side bytes.
- Combine signature and format-specific parser checks.
- Store generated names outside execution-capable locations.

## Key takeaways

Changing a MIME header is not changing file bytes, and therefore MIME validation cannot establish file safety alone.

## Personal observations

To be added.

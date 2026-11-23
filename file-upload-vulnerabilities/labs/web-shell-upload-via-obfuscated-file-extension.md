# Web shell upload via obfuscated file extension

- **Difficulty:** Practitioner
- **Status:** SOLVED

## Vulnerability concept

Filename validation is bypassed through an extension representation that is different from the one used by the application or server parser.

## Root cause

Validation and interpretation follow inconsistent normalization rules. String matching succeeds while the server later resolves or executes a dangerous suffix.

## Technical explanation

Obfuscation may involve casing, multiple suffixes, separators, encoding, or platform-specific name handling. The application and server must agree on the final normalized filename. No exact lab-specific payload is provided here.

See [Filename Security](../filename-security.md).

## What the lab teaches

- Validation must use the final representation.
- Encodings and server-specific parsing may change a filename.
- Extension checks alone cannot establish safety.

## Security implications

The file may be accepted while later interpreted or accessed as server-side executable content.

## Proper remediation

- Generate a fixed extension and opaque name.
- Avoid preserving the original filename for storage.
- Normalize and verify at one authoritative server layer.
- Disable execution in uploads.
- Reject ambiguous or unusual filenames.

## Key takeaways

String transformations must be consistent from upload parsing through storage and serving.

## Personal observations

To be added.

# File Signatures and Magic Bytes

A file signature is a recognizable sequence of bytes associated with a file format. It is sometimes called magic bytes. Examples include format headers and trailers, but no format is guaranteed to be identifiable by a short signature alone.

## MIME, extension, and actual format

These properties answer different questions:

| Property | Meaning | Security limitation |
|---|---|---|
| Extension | Client or server convention | Does not prove content |
| MIME type | Declared or detected media format | Can be wrong or client-controlled |
| File signature | Recognizable bytes | Can be absent, spoofed, or insufficient |
| Parsed structure | Format structure understood by a parser | Parser bugs and embedded content remain relevant |

## Detection limitations

Signature validation can be bypassed by:

- copied headers;
- formats that allow multiple interpretations;
- trailing or embedded data;
- compressed or nested formats;
- malformed structures accepted by one parser;
- parser differences.

A polyglot can be recognized differently by different parsers. Signature checks should therefore be paired with strict parsing and format-specific validation.

## Defensive use

When signature validation is appropriate:

- verify signatures against exact approved formats;
- reject unknown or ambiguous data;
- parse with an up-to-date library;
- enforce dimensions, item counts, and decompressed limits;
- use isolated processing services when practical;
- re-encode or normalize upload formats;
- avoid relying on signature checks for executable-content safety alone.

# Polyglot Files

A polyglot file is valid under more than one format or parser. The formats may share a compatible outer structure or contain data that one parser accepts while another parser interprets differently.

## Why single-property validation fails

An application might inspect only:

- an extension;
- a supplied MIME type;
- a first-byte signature;
- metadata written by a browser.

Another parser may interpret the same bytes differently. A polyglot can therefore satisfy a basic check and be treated as executable or active content later.

The concept relates to the PortSwigger lab **Remote code execution via polyglot web shell upload**. General polyglot behavior is valid knowledge, while exact lab payloads or observations are not claimed in this repository.

## Defensive validation

- choose a single canonical expected format;
- validate full structure with a format-specific parser;
- re-encode or convert to a known-safe representation;
- strip unnecessary metadata and embedded objects;
- isolate document and image processing;
- reject ambiguous structures;
- serve data with controlled headers and no script execution.

Signature and MIME checks reduce uncertainty but do not prove that a file cannot contain embedded active content. Combined controls and safe processing are required.

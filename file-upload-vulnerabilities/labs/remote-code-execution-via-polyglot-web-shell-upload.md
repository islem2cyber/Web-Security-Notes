# Remote code execution via polyglot web shell upload

- **Difficulty:** Practitioner
- **Status:** SOLVED

## Vulnerability concept

A file may satisfy format-oriented validation while remaining valid or interpretable as server-side content.

## Root cause

Validation is based on a limited property rather than the complete format and downstream interpretation. A parser can accept content that an upload filter incorrectly considers safe.

## Technical explanation

A polyglot is recognized differently by different parsers or formats. This lab title indicates that polyglot behavior is relevant to achieving server-side code execution. This repository does not reproduce exact payload bytes or undocumented lab observations.

See [Polyglot Files](../polyglot-files.md).

## What the lab teaches

- Basic extension or signature checks can miss parser ambiguity.
- One file can satisfy multiple format interpretations.
- Server storage and execution configuration remain decisive.

## Security implications

The application may accept content that later executes or is interpreted as active content.

## Proper remediation

- Re-encode or render approved formats into a canonical output.
- Parse full format structure with maintained libraries.
- Strip ambiguous metadata and embedded content.
- Store outside the web root and disable execution.
- Use isolated processing and least privilege.

## Key takeaways

Validation should reduce parser ambiguity and downstream interpretation rather than checking only a single superficial property.

## Personal observations

To be added.

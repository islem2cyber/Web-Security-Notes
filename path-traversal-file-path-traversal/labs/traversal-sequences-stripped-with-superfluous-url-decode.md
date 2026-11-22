# File path traversal, traversal sequences stripped with superfluous URL-decode

- **Difficulty:** Practitioner
- **Status:** SOLVED

## Vulnerability concept

A filter and a later URL-decoding stage can disagree about whether traversal components are present.

## Root cause

The application performs filtering before a later decoding stage, or uses inconsistent canonicalization between web-framework and filesystem layers.

## Technical explanation

If a filter sees an encoded representation, it may not recognize traversal components until another layer decodes the value. The later representation may then reach path resolution.

The exact encoding stages and payload used in the lab are not supplied, and this page does not claim that every application uses the same decoding chain.

## What the lab teaches

- URL decoding must be understood as a processing stage.
- Filters at one stage do not automatically protect later stages.
- Superfluous or double decoding can expose sequences hidden at an earlier stage.
- Validation should use a well-defined canonical value.

## Security implications

Inconsistent decoding can bypass string filters and allow file access outside the intended directory.

## Proper remediation

- Establish a consistent decode-and-normalize policy.
- Reject ambiguous or malformed encoding where appropriate.
- Validate the canonical path at the final filesystem boundary.
- Avoid relying on repeated ad hoc decoding.

## Key takeaways

The security check must inspect the value that reaches the filesystem API, not an earlier representation.

## Personal observations

To be added.

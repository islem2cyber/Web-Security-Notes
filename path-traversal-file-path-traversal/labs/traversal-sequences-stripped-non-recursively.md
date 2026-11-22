# File path traversal, traversal sequences stripped non-recursively

- **Difficulty:** Practitioner
- **Status:** SOLVED

## Vulnerability concept

A one-pass or non-recursive traversal-sequence removal can leave a representation that is still interpreted as a traversal sequence.

## Root cause

The application uses string replacement or parsing that does not account for nested, overlapping, or recombined sequences.

## Technical explanation

When a filter removes a matched substring once, text left by the operation can recombine into a dangerous sequence or normalize to parent-directory behavior. The filter's output is not a canonical filesystem path.

No exact nested sequence or request is claimed here because those lab details were not supplied.

## What the lab teaches

- String replacement is context-sensitive and may not be idempotent.
- Input can contain nested or overlapping patterns.
- One pass can leave another dangerous representation.
- Canonical resolution and containment are stronger boundaries.

## Security implications

The application may accept data that should have been rejected and open a file outside the intended directory.

## Proper remediation

- Remove or reject traversal input instead of recursively rewriting arbitrary filenames.
- Prefer allowlisted identifiers or mapped server paths.
- Canonicalize the final path and verify it remains under a fixed trusted base.

## Key takeaways

Repeated replacements, alternative representations, and final normalization can invalidate a denylist-only design.

## Personal observations

To be added.

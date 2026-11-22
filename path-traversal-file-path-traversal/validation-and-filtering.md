# Path Validation and Traversal Sequence Filtering

Path validation must reflect the application's intended file location, not only the presence or absence of selected text.

## Basic traversal sequences

The sequence `..` and a separator can represent movement to a parent directory. Examples in technical explanations may use `/` or `\` depending on platform.

This representation is conceptual. It is not evidence that a specific lab used a particular payload or filename.

## Absolute-path bypass

When an application rejects traversal sequences but then passes an absolute path to a filesystem API, a value beginning at the filesystem root may bypass the sequence filter entirely.

The correct question is not "Does the value contain `../`?" It is "Does the final resolved path remain in the trusted directory?"

## Non-recursive filtering

A replacement such as removing `../` once may leave nested or overlapping sequences:

```text
Illustrative input: ....//secret
One-pass removal:   .../secret
```

The exact remaining string depends on replacement rules and separator handling. A more direct example is a non-recursive filter that removes a recognized sequence from `....//`. The neighboring text can recombine or normalize into a traversal sequence.

The lesson is that string replacement is not a reliable security boundary. Nested, overlapping, encoded, or platform-specific representations can differ from what a filter expects.

## Prefix validation

Validation of the beginning of a path is demonstrated by checks that accept values beginning with a trusted directory or its text representation.

A beginning check can fail because:

- `..` components appear after the accepted prefix.
- Absolute and relative paths are interpreted differently.
- The prefix string is not a directory boundary.
- Separators or case differences alter filesystem interpretation.
- A symlink or junction redirects beneath the accepted-looking path to another location.

Use canonical containment rather than a textual prefix alone.

## File-extension validation

Checking a filename's ending can ensure that a file has an expected suffix, but it does not ensure that the file is inside an intended directory. A valid-looking suffix can coexist with an unsafe path.

String extension checks can also be bypassed or confused by encoding, trailing components, alternate separators, or implementation-specific string termination. The fix is to map an identifier or allowlist filenames and resolve files against a fixed trusted base.

## Allowlisting versus denylisting

An allowlist defines what is accepted. A denylist attempts to enumerate dangerous values.

Allowlisting is generally stronger for filenames because:

- The set of legitimate names is usually small and predictable.
- Application code can return a known server-side path.
- Unknown or malformed values fail safely.
- Filesystem syntax does not need to be guessed by the filter.

Where arbitrary nested paths are required, canonicalize and enforce directory containment; do not rely on denylisting traversal strings.

Continue with [Path normalization](./path-normalization.md), [Null byte injection](./null-byte-injection.md), and [Remediation](./remediation.md).

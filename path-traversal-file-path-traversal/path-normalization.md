# Path Normalization

Path normalization is the process of interpreting path components and producing a form in which traversal and separator semantics have been resolved. Canonicalization goes further conceptually by representing the same location in a consistent, final filesystem form.

## Relative and absolute resolution

For a base directory such as `/srv/app/files`, a relative value can be resolved:

```text
/srv/app/files/./images/../images/report.txt
```

Normalization may produce a path equivalent to:

```text
/srv/app/files/images/report.txt
```

This process can follow `.` and `..` components. The exact operation and return format depend on the programming language and filesystem API.

## Validation order matters

Validating before normalization can accept data that later resolves outside the intended directory:

```text
Raw value:       safe/../../private.txt
Precheck:        looks to begin in "safe"
Final resolution:/srv/app/private.txt
```

The exact behavior depends on implementation. The security principle is that validation must apply to the final path or to a canonical representation produced by the filesystem API.

## Final boundary check

A secure design resolves the candidate path against a fixed base directory and verifies its final location remains below that base. A common conceptual test is:

```text
base  = canonicalize(trusted_base)
candidate = canonicalize(join(base, server-controlled_or_mapped_value))
allow only when candidate is equal to base or lies inside base
```

Filesystem APIs differ in their handling of symlinks, nonexistent files, case sensitivity, and platform root syntax. Boundary checks must use the actual runtime and filesystem semantics.

## Prefix checks are not boundaries

Checking that a string starts with `/srv/app/files/` is not sufficient if path text is not first normalized or if an invalidly named sibling is considered:

```text
/srv/app/files/../secrets.txt
/srv/app/files-backup/report.txt
```

The first contains traversal syntax; the second merely shares a string prefix but is a different directory. A canonical containment check is safer than an unparsed string-prefix check.

## Operating-system differences

- Unix-like systems commonly treat `/` as a separator and may have case-sensitive names.
- Windows commonly recognizes `\` and `/` in many APIs, can use drive-relative paths, and may have case-insensitive and device-name behavior.
- Symlinks and junctions can point outside a visually contained directory.
- Filesystem APIs may prohibit or resolve nonexistent paths differently.
- URL percent-decoding and filesystem canonicalization are separate stages unless the application intentionally combines them.

Review [Encoding and decoding](./encoding-and-decoding.md) for the interaction between web decoding and filesystem normalization.

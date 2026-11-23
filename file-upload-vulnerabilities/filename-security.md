# Filename Security

Filenames are both data and path components. The original upload filename should not be trusted as a storage name or path.

## Path traversal

If an application joins a user-controlled filename to an upload directory, values containing `../`, absolute paths, separators, or alternate encodings may influence the destination. Unsafe handling can:

- write outside the upload directory;
- overwrite an existing file;
- expose application or configuration files;
- create executable files in a web-accessible location.

Cross-link: [Path Traversal fundamentals](../path-traversal-file-path-traversal/fundamentals.md) and [Path Traversal remediation](../path-traversal-file-path-traversal/remediation.md).

## Safe storage design

- construct the destination from a server-generated filename;
- use a fixed directory selected by the application;
- ensure the directory is canonical;
- resolve and verify the final path remains inside the intended base;
- reject path separators and control characters;
- avoid using upload names as filesystem or URL identifiers;
- create a collision-resistant mapping to the original display name if needed;
- remove or deny symbolic-link-based destination changes.

The server-generated name should not contain spaces or user text. Store the original display name as data only when needed and render it safely.

## Basename and normalization

PHP `basename()` removes directory components, but it is not a complete defense. Correctly validate the entire path and storage workflow. URL decoding, Unicode normalization, server-specific separator handling, and filesystem aliases can differ from application assumptions.

Do not generate a destination based on the client filename and then apply only a blacklist of `../`. Generate an opaque name and fixed base directory instead.

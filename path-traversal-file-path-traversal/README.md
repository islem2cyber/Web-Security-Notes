# Path Traversal (Directory Traversal / File Path Traversal)

Personal notes and PortSwigger Web Security Academy lab progress for Path Traversal vulnerabilities.

## Learning progress

**6 of 6 labs completed:**

- Apprentice: 1/1 completed
- Practitioner: 5/5 completed
- Postponed: 0
- Overall completion: 6/6 (100%)

These notes distinguish general path-handling knowledge from verified personal lab observations. The lab titles and statuses below are supplied progress facts; exact payloads, requests, and screenshots have not been provided.

## Repository structure

- [Lab tracker](./lab-tracker.md)
- [Path traversal fundamentals](./fundamentals.md)
- [Path normalization](./path-normalization.md)
- [Encoding and decoding](./encoding-and-decoding.md)
- [Validation and filtering](./validation-and-filtering.md)
- [Null byte injection](./null-byte-injection.md)
- [Testing methodology](./testing-methodology.md)
- [Remediation](./remediation.md)
- [Lab documentation](./labs/)

## Learning objectives

- Explain how file paths are constructed and when user-controlled values can escape an intended directory.
- Distinguish directory traversal terminology from the underlying file-path access flaw.
- Understand canonicalization, URL decoding, filtering, and validation-order problems.
- Identify prefix, extension, replacement, absolute-path, and null-byte bypass concepts.
- Apply a repeatable authorized testing workflow.
- Replace string-based checks with allowlists, canonical path checks, and least-privilege access.

## Progress summary

| Difficulty | Completed | Total | Postponed |
|---|---:|---:|---:|
| Apprentice | 1 | 1 | 0 |
| Practitioner | 5 | 5 | 0 |
| **Total** | **6** | **6** | **0** |

The exact titles and statuses are in the [Path Traversal lab tracker](./lab-tracker.md).

## References

- [PortSwigger Web Security Academy - Path traversal](https://portswigger.net/web-security/file-path-traversal)
- [OWASP - Path Traversal](https://owasp.org/www-community/attacks/Path_Traversal)
- [OWASP - Input Validation Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Input_Validation_Cheat_Sheet.html)
- [Python - pathlib: Pure paths and common operations](https://docs.python.org/3/library/pathlib.html)
- [Python - os.path - Common pathname operations](https://docs.python.org/3/library/os.path.html)

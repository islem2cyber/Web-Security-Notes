# File Upload Vulnerabilities

Personal notes and PortSwigger Web Security Academy lab documentation for File Upload Vulnerabilities.

## Learning progress

**6 of 7 labs completed:**

- Apprentice: 2/2 solved
- Practitioner: 4/4 solved
- Expert: 0/1 completed; 1 implementation pending
- Postponed: 0
- Overall completion: 6/7

The Expert lab, **Web shell upload via race condition**, is documented as `IMPLEMENTATION PENDING`. It is not marked solved or failed because the concurrent request mechanism was not implemented independently. The race-condition concept is understood, and the lab remains to be reproduced after learning request concurrency and timing coordination.

## Repository structure

- [Lab tracker](./lab-tracker.md)
- [File upload fundamentals](./fundamentals.md)
- [File validation](./file-validation.md)
- [Content-Type and MIME](./content-type-and-mime.md)
- [File signatures and magic bytes](./file-signatures-and-magic-bytes.md)
- [Filename security and path traversal](./filename-security.md)
- [Storage and execution](./storage-and-execution.md)
- [Polyglot files](./polyglot-files.md)
- [Race conditions](./race-conditions.md)
- [Testing methodology](./testing-methodology.md)
- [Remediation](./remediation.md)
- [Programming knowledge required](./programming-language-context.md)
- [Lab documentation](./labs/)

## Progress summary

| Difficulty | Solved | Implementation pending | Total |
|---|---:|---:|---:|
| Apprentice | 2 | 0 | 2 |
| Practitioner | 4 | 0 | 4 |
| Expert | 0 | 1 | 1 |
| **Total** | **6** | **1** | **7** |

Postponed: 0.

## Core topic

A file upload vulnerability arises when application handling of uploaded data is not restricted to a safe, expected file type and a controlled storage location. Validation should run on the server. The generated server-side filename, storage location, processing workflow, and execution configuration determine the actual impact.

An uploaded executable file does not automatically execute. It becomes dangerous only when the application exposes or processes it under conditions that permit the relevant interpreter or server-side behavior.

## References

- [PortSwigger - File upload vulnerabilities](https://portswigger.net/web-security/file-upload)
- [OWASP - File Upload Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/File_Upload_Cheat_Sheet.html)
- [PHP - Handling file uploads](https://www.php.net/manual/en/features.file-upload.php)
- [PHP - Fileinfo](https://www.php.net/manual/en/book.fileinfo.php)
- [MDN - File API](https://developer.mozilla.org/en-US/docs/Web/API/File_API)
- [MDN - FormData](https://developer.mozilla.org/en-US/docs/Web/API/FormData)
- [MDN - Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API)
- [RFC 9110 - HTTP Semantics, Content-Type](https://www.rfc-editor.org/rfc/rfc9110.html#name-content-type)

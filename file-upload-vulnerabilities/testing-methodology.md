# Testing Methodology

Use this workflow only for systems you are authorized to test.

1. **Identify file upload endpoints.**
   - Map HTML forms, API operations, profile features, import routes, and document workflows.
   - Record expected formats and required authorization.

2. **Understand the expected file types.**
   - Determine the documented formats and whether image transformation or document processing occurs.

3. **Inspect the HTTP upload request.**
   - Identify `multipart/form-data`, field names, filenames, size, and declared `Content-Type`.

4. **Identify client-side and server-side validation.**
   - Browser checks are usability controls. Test server behavior directly using authorized HTTP requests.

5. **Examine Content-Type handling.**
   - Determine whether declared MIME values affect acceptance and whether server-side bytes are checked.

6. **Examine extension validation.**
   - Review allowlist versus blacklist behavior, normalization, case, and server-specific mappings.

7. **Examine filename handling.**
   - Test path components and filename transformations only within an authorized lab. Confirm whether final storage names are server-generated.

8. **Examine storage and retrieval.**
   - Determine whether files are in the web root, outside it, or behind an application route. Check authorization and content headers.

9. **Determine processing or execution.**
   - Check whether the server executes scripts, transforms documents, parses archives, or renders content in a browser context.

10. **Test validation weaknesses.**
    - Use the most specific supported lab concept. Avoid exact payload claims or unsupported observations in personal notes.

11. **Examine race conditions when relevant.**
    - Sequential requests cannot prove a concurrent window. Use coordinated authorized requests and avoid assuming a timing issue exists.

12. **Document impact and remediation.**
    - Record root cause, verified behavior, affected configuration, and specific defensive changes.

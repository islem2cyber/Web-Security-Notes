# Path Traversal Testing Methodology

Use this methodology only in explicitly authorized lab or test environments.

## 1. Identify file-retrieval endpoints

Look for features that display images, downloads, reports, templates, documents, source maps, imported files, or generated exports. These endpoints often pass a filename or path parameter to file-access code.

This step defines likely candidates; a file-download feature does not prove a traversal flaw.

## 2. Locate user-controlled path values

Trace values in query parameters, form fields, route segments, headers, cookies, or API objects. Determine whether the application combines one value with a directory or template.

The purpose is to identify trust boundaries and the route from request input to filesystem access.

## 3. Understand the expected directory structure

Identify a normal file, the expected base directory, and the server-side purpose of the parameter. This supports interpretation of later response differences.

Do not assume a lab's filesystem layout from the web UI alone.

## 4. Inspect normal file access behavior

Request a known valid file and record the response, status, content type, errors, and whether a missing file differs from an existing file.

Baseline behavior makes it easier to determine whether a test reaches a file or is merely producing an application error.

## 5. Identify validation and filtering

Observe whether the application rejects separators, parent components, absolute paths, extensions, or encoded values. Test only in a scope where traversal is permitted.

Distinguish a displayed client-side check from server-side enforcement.

## 6. Consider decoding and canonicalization

Determine whether the parameter may pass through URL decoding, framework routing, proxies, or application-specific parsing. Repeated or inconsistent decoding can change a value between validation and file access.

Do not assume double encoding is present unless the observed system shows the behavior.

## 7. Test traversal handling in the authorized lab

Vary path forms only as necessary to test the relevant control. Compare normal, missing, and sensitive file behavior.

Use a minimal lab-approved workflow and stop if the result is outside scope.

## 8. Observe error and response differences

Record status codes, response bodies, headers, timing, cache behavior, and redirects. Do not treat a generic error or identical response as proof of successful file access.

## 9. Determine whether the intended boundary was escaped

Verify that a final file was outside the application's trusted base directory and was accessible with the application's privileges.

Path syntax alone does not establish impact.

## 10. Document impact and remediation

Record what was observed, what was not observed, and the permissions of the application process. Propose allowlisting, canonicalization, containment checks, and least privilege.

## Evidence discipline

- Never claim a payload was used unless it was personally recorded.
- Mark exact solution details as "To be added" until supplied.
- Separate illustrative examples from verified observations.
- Avoid screenshots or request captures that contain unintended sensitive data.

# Storage and Execution

Upload security depends on storage location, serving behavior, and server execution configuration.

## Web-accessible storage

Serving an upload directly from the web root allows a browser or HTTP client to retrieve it. This is expected for public images or documents, but active content or scripts require additional controls.

Recommended designs include:

- storage outside the web root;
- a dedicated download route that authorizes access;
- strict `Content-Disposition` and `Content-Type` handling;
- disabled script execution in upload directories;
- object storage with controlled public access;
- least-privilege filesystem permissions.

Execution requires more than a web-accessible name. The server must map the file type or location to an interpreter and have permission to process it. These conditions should not be assumed or fabricated in a lab; they must be verified under the authorized test configuration.

## Web shells

A web shell is server-side code exposed through HTTP or another application entry point. In a vulnerable configuration, a web shell upload can permit server-side code execution under the application or web-server account.

The safe defensive model is:

1. keep uploaded content outside script-capable locations;
2. disable PHP, CGI, SSI, and other interpreters in upload directories;
3. use generated names and fixed extensions;
4. run the application with least privilege;
5. isolate processing and prevent shell or process invocation;
6. monitor upload and execution activity.

**Illustrative and non-operational example:**

```text
uploaded.png
  |
  +-- stored outside web root
  +-- served by an authorization-aware download controller
  +-- Content-Type set explicitly
  +-- interpreted only as binary/file data
```

## Stored XSS

Uploaded HTML or SVG can create stored XSS if later rendered or opened in a browser context that executes it. Upload controls must account for browser interpretation, not only server-side script execution.

## Atomic operations

Validate and normalize before publication. A file should not become readable in a live directory while its final checks or renaming are incomplete. Use secure temporary storage, perform processing offline or in isolation, and move or publish the file atomically when complete.

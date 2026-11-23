# File Upload Fundamentals

A file upload vulnerability is an unsafe application state involving attacker-controlled file data or metadata. The data may bypass validation, be stored in an unintended location, overwrite another file, or later be interpreted as executable or active content.

## Upload flow

1. A browser or client constructs an HTTP `multipart/form-data` request.
2. The server parses the request and creates temporary or application-managed file data.
3. Server-side code examines or processes the upload.
4. The application chooses a destination and filename.
5. The file is stored, processed, returned, or made available to users.

In PHP applications, `$_FILES` contains metadata from the parser for each uploaded field. `move_uploaded_file()` may move a valid HTTP upload from its temporary location.

## Client-side and server-side validation

Client-side validation in HTML or JavaScript can improve usability and reject obvious mistakes before a request is sent. It is not a security boundary because an attacker can:

- modify a browser or JavaScript request;
- send a direct HTTP request;
- bypass or alter HTML attributes;
- change request fields and headers.

Server-side validation is required. It should be applied to the data and metadata actually received, with authorization and resource limits enforced independently.

## Attack surfaces

Relevant upload data and behavior include:

- original filename and path components;
- extension and filename transformations;
- declared and actual media type;
- file size;
- file signature;
- archive contents;
- temporary storage;
- destination path;
- file-processing libraries;
- file retrieval and download headers;
- server configuration and script interpreters.

## Execution relationship

Uploading a file and executing a file are separate states. The impact depends on:

- the web root and storage location;
- interpreter mappings and server configuration;
- filename and extension handling;
- execution permissions;
- authentication and authorization;
- whether files are served or processed.

A web shell is a server-side script that can process requests and execute server-side operations in a vulnerable configuration. Modern or correctly configured servers may store or serve it without interpreting it.

## Potential impacts

- unauthorized file access or disclosure;
- stored malicious content;
- server-side code execution under vulnerable configurations;
- stored XSS when active content is accepted in a context in which the browser executes it;
- denial of service through size, decompression, or processing abuse;
- overwrite or manipulation of application or configuration files;
- data destruction or modification through unsafe processing.

Not every uploaded executable file is executable. Defensive systems should remove or reduce each of the relevant conditions rather than assume one upload control solves all risks.

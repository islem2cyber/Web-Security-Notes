# Content-Type and MIME Type Validation

HTTP media types identify the format of a representation. `Content-Type` may appear on a request or response and is commonly formatted as `type/subtype`, sometimes with parameters.

**Illustrative request example only:**

```http
POST /upload HTTP/1.1
Host: example.invalid
Content-Type: multipart/form-data; boundary=ExampleBoundary

--ExampleBoundary
Content-Disposition: form-data; name="file"; filename="image.bin"
Content-Type: image/png

<file bytes>
--ExampleBoundary--
```

The multipart part's `Content-Type` is metadata supplied by the client. Browsers may infer or assign it, but it is not authoritative proof of the bytes.

## MIME types and browser behavior

MIME types help browsers and servers decide how to present or process a resource. They do not make bytes of that format. A server may use:

- the part or response `Content-Type`;
- the filename extension;
- a server magic/type database;
- downstream parsing libraries;
- content-disposition handling.

The browser and server are separate consumers and may use different rules.

## Why declared MIME types are unsafe

An attacker can change `Content-Type` independently of file content. Validation based only on a multipart declaration can accept HTML, script, or executable bytes labeled as an image. Likewise, a valid image may contain embedded active content in a format accepted by another parser.

## Server-side strategy

Server-side validation can inspect the received bytes and approved formats. Prefer a file-type library and format parser with safe settings. Use the MIME result as one signal, not the only decision.

A robust workflow:

1. verify upload and size limits;
2. inspect bytes for an approved signature;
3. parse only allowed formats;
4. remove or reject metadata that may create active content;
5. re-encode or re-render formats when safe and practical;
6. generate a server filename;
7. store outside executable web locations.

Set response `Content-Type`, `X-Content-Type-Options: nosniff`, and `Content-Disposition` according to application requirements. These headers support safe delivery but do not repair the server-side file-validation failure.

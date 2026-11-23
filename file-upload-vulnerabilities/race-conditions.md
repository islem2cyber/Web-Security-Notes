# Race Conditions in File Uploads

A race condition occurs when the result of an operation depends on the relative timing of concurrent execution. In upload workflows, validation and publication may occur as separate steps.

## TOCTOU

Time-of-Check to Time-of-Use (TOCTOU) means a file or path is checked, then later used after the state has changed.

For example:

```text
upload temporary file
  -> validation starts
  -> file is briefly published
  -> request to the file is processed
  -> validation/deletion is completed
```

The relevant concern is not merely that a file passes validation incorrectly. It is that there may be a timing window in which the application exposes or processes a file before a check or cleanup is complete.

## Concurrent requests

Concurrent HTTP requests overlap in time instead of being completed strictly one after another. A sequential test may always check the file after deletion or cleanup and appear safe.

The Expert lab **Web shell upload via race condition** requires:

- concurrent HTTP requests;
- timing coordination;
- understanding of upload and processing/deletion windows.

This documentation does not claim that the practical concurrent attack was completed.

## Status

**IMPLEMENTATION PENDING**

General understanding:

- upload and processing/deletion can overlap;
- timing-dependent behavior may be missed by sequential requests;
- concurrency is needed to reproduce the window.

Remaining learning objectives:

- understand concurrent request execution;
- coordinate upload and retrieval requests;
- learn a timing and orchestration workflow using an appropriate authorized testing tool;
- reproduce the lab independently.

## Defensive strategies

- perform validation before durable publication;
- use secure, application-only temporary storage;
- publish files atomically;
- keep cleanup and validation in a single controlled workflow;
- never rely on a filename appearing only briefly as a security boundary;
- isolate processing so untrusted content cannot execute;
- use locks or reservations when shared state requires it;
- monitor and log the upload lifecycle.

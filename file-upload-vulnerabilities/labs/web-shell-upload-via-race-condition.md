# Web shell upload via race condition

- **Difficulty:** Expert
- **Status:** IMPLEMENTATION PENDING

## Vulnerability concept

The upload workflow exposes or processes a file during a validation or deletion window, and the outcome depends on overlapping operations.

## Root cause

Time-of-Check to Time-of-Use occurs when a file is checked and later used without an atomic, isolated workflow. Temporary publication can overlap processing or deletion.

## Technical explanation

The general race is:

```text
upload
  -> temporary or live file publication
  -> overlapping processing/check
  -> cleanup or deletion
  -> a concurrent request during the timing window
```

Sequential requests may miss the window because cleanup is complete before later requests reach the file. Concurrency changes the relative timing of upload and retrieval/processing. No exact request captures or timing values are available.

See [Race Conditions](../race-conditions.md).

## What I understood

- A race condition depends on concurrent operations and a timing window.
- Upload validation and cleanup/deletion are separate workflow phases.
- Sequential requests may not reproduce the condition.
- The technique requires coordinating multiple HTTP requests.

## What remains to be learned

- Implement practical concurrent HTTP requests.
- Coordinate upload and follow-up requests.
- Understand authorized tool orchestration and timing.
- Reproduce the lab independently and document exact verified observations.

## Security implications

The impact may include temporary access to unvalidated or executable content before secure cleanup. Real impact depends on storage and server execution configuration.

## Proper remediation

- Publish only after validation and processing complete.
- Use secure application-only temporary storage.
- Move files atomically to their final location.
- Isolate processing and disable execution.
- Avoid TOCTOU conditions in shared filesystem state.
- Add lifecycle monitoring and cleanup.

## Key takeaways

This lab remains to be implemented. It is not marked solved or failed. It requires practical concurrency and timing coordination.

## Personal observations

To be added.

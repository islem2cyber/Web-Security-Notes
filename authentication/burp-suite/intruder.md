# Burp Suite Intruder

Intruder can vary marked request positions and help analyze responses in authorized labs. Use it only within scope, with test accounts, a conservative request budget, and a clear reason for each test. It is not appropriate to run credential guessing against real users or services without explicit authorization.

## Basic concepts

- **Attack positions:** request locations selected for controlled substitution. Mark only values relevant to the hypothesis; protect session and anti-CSRF values.
- **Payload positions:** places where Intruder inserts configured values.
- **Payload types:** ways to provide or derive test values. Choose benign, bounded values appropriate to the lab.
- **Payload processing:** transformations applied before a value is sent. Review them carefully; unexpected encoding can change what the server receives.
- **Grep - Match:** flags response text or patterns for comparison.
- **Grep - Extract:** extracts selected response content to compare changing fields, such as a token or message.
- **Response analysis:** compare status, length, timing, redirects, and relevant content; dynamic fields can create noise.

## Attack types

| Type | How positions are populated | Typical conceptual use |
|---|---|---|
| Sniper | One payload set is applied to each position in turn | Isolate the effect of one candidate value at a time. |
| Battering ram | The same payload is inserted into all selected positions | Test whether repeated occurrences of one value behave consistently. |
| Pitchfork | Multiple payload sets advance together by matching index | Test aligned pairs of values where each row is a meaningful pair. |
| Cluster bomb | Payload sets are combined across positions | Explore combinations; the request count can grow quickly, so use only a small, justified, bounded set. |

The correct choice depends on the test hypothesis. Avoid large combinations or high request volumes. When authentication state changes per request, ordinary payload configuration may be insufficient; use the [session-handling learning reference](./macros-and-session-handling.md) and validate a single request before any bounded run.

## Responsible response analysis

Normalize or account for dynamic timestamps, tokens, and session values before treating differences as meaningful. Confirm a pattern with a small repeatable test. Stop if responses indicate lockout, service degradation, an unexpected identity, or out-of-scope behavior. Record the scope, rate, test account, and observed effects.

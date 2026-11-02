# Discovery Workflow for XSS Analysis

This workflow is intended as a repeatable process for building manual discovery skills in a controlled lab or application context.

## 1. Reconnaissance

Start by identifying the target surface and possible user-controlled input locations.

Look for:
- URL parameters
- form fields and POST data
- fragments and hash values
- cookies or browser state
- referrer values
- data returned from APIs or JavaScript state

The goal is not to guess a payload immediately but to identify where untrusted data enters the application.

## 2. Input discovery

Confirm that the input is attacker-controlled and reaches the application in a way that matters.

Ask:
- Where does the data come from?
- Can a user influence it?
- Is the value reflected, stored, or used only in client-side logic?

## 3. Reflection analysis

Check whether the value is returned in an HTTP response or rendered in the DOM.

Look for cases where the app includes data in:
- HTML content
- attributes
- JavaScript strings
- URL parameters
- template expressions

This is where the first clues about the injection context usually appear.

## 4. Source identification

Map the value to a specific source in the client or server logic.

Examples include:
- `location.search`
- `location.hash`
- `window.name`
- `document.referrer`
- request parameters
- stored database values

A source is relevant only if the value can be influenced by an attacker in the active scenario.

## 5. Sink identification

Find the operation that processes or inserts the data into a browser-interpreted output path.

Common sinks include:
- `innerHTML`
- `outerHTML`
- `document.write()`
- `eval()`
- URL assignments
- jQuery DOM APIs
- template evaluation paths

This step answers: where does the value become active content?

## 6. Data-flow tracing

Trace how the data moves from source to sink.

Document:
- where it enters
- whether it is encoded or transformed
- whether it is stored first
- what function or property handles it
- whether it is concatenated into HTML, JavaScript, or a URL

A vulnerable flow often shows a clear path from untrusted input to an execution sink.

## 7. Context analysis

Evaluate the surrounding syntax and grammar.

Ask:
- Is the value inside an HTML element?
- Is it inside an attribute?
- Is it in a JavaScript string or template literal?
- Is it part of a URL or event handler?

The output context matters more than the payload alone.

## 8. Security-control analysis

Check what protections are present.

Examples:
- encoding rules
- filter or WAF restrictions
- CSP headers
- framework sandboxing
- browser protections

Do not assume the app is vulnerable simply because a sink exists. Security controls may limit or block the exploit path.

## 9. Safe exploitability verification

Use a minimal proof-of-concept to confirm the issue, but keep the test controlled and relevant.

Examples:
- `alert(1)` in authorized labs
- a harmless script to confirm execution
- evidence that the untrusted value reaches the sink as expected

This step should confirm the root cause without over-claiming impact or scope.

## 10. Impact assessment

Assess the actual impact.

Consider:
- which users are affected
- whether the issue is stored or reflected
- whether the behavior is client-side only or server-connected
- whether the same vulnerability can affect authentication or session state
- whether the browser and application context materially increase risk

Impact must be grounded in what the application actually does, not just in the theoretical existence of a bug.

## 11. Root-cause documentation

Write down the actual root cause in plain language.

Good notes describe:
- the source
- the sink
- the context
- the unsafe flow
- the trigger condition
- the security control involved
- why the application is or is not vulnerable

## 12. Mitigation recommendations

Finally, document the fix in terms of prevention.

Practical defenses include:
- validating input where appropriate
- encoding output according to the exact context
- avoiding dangerous sinks with untrusted values
- using CSP as a defense-in-depth control
- validating user-controlled data before it reaches browser-driven execution paths

## Summary

Manual XSS discovery is a process of connecting outputs to the browser's interpretation model. The goal is not to memorize payloads, but to learn how to identify the source, the sink, the context, and the security controls that make the issue exploitable or harmless.

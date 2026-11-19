# Referer Validation

The Referer (HTTP spelling) or referrer header identifies a referring resource in requests made with a browser. Applications may use it to infer the previous page or origin.

## Header behavior

Browsers do not send or truncate Referer headers in every context, and privacy settings or policy headers can suppress or modify them. The server should therefore treat the header as optional and untrusted client metadata.

## Referer as a CSRF defense

An application can compare a Referer with an allowlisted expected origin or host. It may also reject sensitive actions when a meaningful Referer is absent or mismatched.

This control is less robust than a cryptographically random, session-bound CSRF token.

## Header presence mistakes

Accepting every request when the Referer is missing turns a validation policy into an explicit bypass. A common unsafe implementation does roughly the following:

```text
if Referer is present and does not match expected origin: reject
else: accept
```

A missing header is not evidence of a same-origin request. Privacy controls or direct navigation may legitimately remove it.

## Weak validation

Common bypasses include substring matches, suffix-only checks, and allowlisting the wrong host component. An attacker-controlled host such as `attacker.example` might satisfy a weak `example` or `domain.com` match.

Prefer parsing the URL and comparing the scheme, host, and optionally port under a strict origin policy. Account for canonicalization and proxy rewriting before relying on the value.

## Use as defense-in-depth

Referer validation should complement, not replace, CSRF tokens or framework protections. It can detect mismatched contexts and block unsupported browser behavior, but it cannot establish server-side intent or prevent a browser that correctly sends an authorized-looking header.

# Encoding and Decoding

Web path values commonly pass through URL parsing and decoding before application code uses them. The same string may be interpreted differently at the web server, framework, proxy, or application layer.

## URL percent-encoding

Percent-encoding represents a byte or character in a URL, for example:

```text
%2e%2e%2f
```

In a system that decodes the value once, this can become a traversal sequence. In another system, it may remain encoded until a later component decodes it.

This is an illustrative representation, not a record of a payload used in the provided labs.

## Decoding stages

Decoding may occur in several places:

1. HTTP server or reverse proxy parsing.
2. Web framework routing or parameter decoding.
3. Application-specific decoding or base64 processing.
4. Filesystem library interpretation, if it treats encoded characters specially.

Not every application performs every stage. Double encoding means a value has been encoded before a second decoding stage interprets the result; `multiple decoding stages` or `superfluous URL decoding` can therefore produce a difference between filter input and filesystem input.

## Inconsistent layer behavior

Suppose a filter executes on the first decoded value while the application later decodes the value again. A traversal sequence may be visible only to the later component and reach a filesystem API.

In contrast, consistent decoding followed by normalization can give the application a canonical value for validation.

## Filtering at the wrong stage

A denylist or replacement operation applied before URL decoding may miss a traversal sequence introduced later. A filter applied after one decoding stage may miss a sequence introduced by another stage.

The defensive goal is not simply to add repeated decoding. Repeated decoding can mutate legitimate values and may hide ambiguity rather than resolve it. A design should define:

- Which components decode URL and application parameters.
- When request values become filesystem paths.
- Whether separators and traversal components are normalized.
- How malformed or ambiguous encoding is rejected.
- Whether final canonical paths remain inside the trusted base.

See [Validation and filtering](./validation-and-filtering.md) for practical security boundaries.

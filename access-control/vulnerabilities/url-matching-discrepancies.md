# URL-Matching Discrepancies

## Overview

URL-matching discrepancies occur when different layers interpret the same request differently. Routing, authorization, proxy rewriting, or framework path normalization may not agree on whether a URL is protected or matches a restricted pattern.

## Common discrepancy types

### Case sensitivity differences

Some systems normalize paths differently:

- `/Admin` vs `/admin`
- `/Users` vs `/users`

If the authorization layer or web server interprets the case differently, a bypass may arise when one layer treats the path as public while another treats it as restricted.

### Trailing slash differences

Some frameworks treat `/admin` and `/admin/` as the same route, while others do not. If authorization rules are attached to one form but not the other, a bypass may appear.

### Path normalization

Path normalization may collapse or rewrite sequences such as:

- `//`
- `../`
- encoded slashes
- duplicate separators

If the application uses a permissive route matcher on one side and a strict one on the other, the authorization boundary may be inconsistent.

### File extension and suffix matching

Frameworks may differ in how they match file extensions or suffix patterns. An authorization rule designed for `/admin` may not apply to `/admin/` or `/admin.jsp` depending on configuration.

## Spring `useSuffixPatternMatch`

A historical example is the Spring framework configuration `useSuffixPatternMatch`, which could affect whether suffixes such as `.json`, `.html`, or `.action` were considered part of the route. Before Spring 5.3, the behavior could be permissive in ways that affected path matching and route protections. This is an example of how framework defaults can create authorization mismatches.

The important point is not the framework itself, but the fact that authorization and routing are not always interpreted identically.

## Why this matters for access control

If the router allows a request but the access-control layer does not properly review it, the application can create a bypass. A path may appear protected in one component and unprotected in another due to different normalization or matching logic.

## Defensive principle

Authorization should not rely on a route pattern that can differ between layers. Enforce policy on the canonical server-side resource and the final normalized request context, not on an assumption that all components interpret paths the same way.

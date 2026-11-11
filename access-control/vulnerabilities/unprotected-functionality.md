# Unprotected Functionality

## Overview

Unprotected functionality is the exposure of protected features without proper authorization. This often affects admin areas, hidden management functions, support tools, configuration pages, or privileged actions that should be gated.

## Common examples

- Administrative endpoints exposed without a role check.
- Direct navigation to restricted pages.
- Hidden links that rely on UI secrecy instead of authorization.
- Functionality protected only by `display: none` or absence from navigation.

## Hidden links do not provide authorization

A route is not protected simply because it is not listed in the menu or hidden from the default UI. Attackers can discover hidden endpoints through:

- Source code.
- HTML comments.
- JavaScript references.
- Sitemap or robots files.
- Network inspection.
- Brute-force or enumeration.

The server must enforce access control regardless of whether a user can discover the link visually.

## Unpredictable URLs do not provide authorization

The same principle applies to URLs that appear random or “unpredictable.” Security by obscurity is not a substitute for access policy enforcement. An attacker with a guessed or discovered path may still be able to access the functionality if the server does not check the identity and role.

## Information leakage

Unprotected functionality may also reveal sensitive information through:

- JavaScript files.
- HTML comments and templates.
- Source code or static assets.
- Search result pages.
- `robots.txt` and application maps.
- Error messages or debug output.

This is especially relevant in applications where admin functionality is exposed but not gated properly.

## Discovery and wordlists

When testing in an authorized environment, review the application for administrative pathways and keywords common to restricted features, including:

- `admin`
- `manage`
- `settings`
- `users`
- `logs`
- `export`
- `debug`
- `backup`
- `config`

These paths should be evaluated under the same authorization policy as the rest of the application.

## Defensive principle

The authorization decision must be server-side and role-based. Users should never be allowed to reach a privileged endpoint simply because they can guess, inspect, or infer its URL.

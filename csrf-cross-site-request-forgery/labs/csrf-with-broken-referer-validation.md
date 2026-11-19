# CSRF with broken Referer validation

- **Difficulty:** Practitioner
- **Status:** SOLVED

## Vulnerability concept

The application performs weak or incorrectly scoped Referer validation that an attacker-controlled value can satisfy.

## Root cause

The server parses or compares the Referrer insufficiently, for example by using an unsafe substring or suffix check.

## Technical explanation

A hostname can contain an expected application string without being under that application's authority. Weak checks such as `contains("example.com")` may accept `attacker-example.com` or an attacker-controlled subdomain.

Exact lab payloads are not reproduced here.

## What the lab teaches

- Referer is client-controlled metadata.
- URL parsing must compare origins or trusted host components accurately.
- Domain syntax and canonicalization must be considered.
- Referer validation alone is not strong CSRF protection.

## Security implications

An attacker with a host satisfying the weak check may trigger a state-changing request in an authenticated browser.

## Proper remediation

- Use strict origin parsing and allowlisted trusted origins.
- Avoid substring and suffix allowlists.
- Prefer `Origin` validation or framework CSRF tokens for server-side enforcement.
- Consider proxy rewriting and privacy policy before defining behavior.

## Key takeaways

Broken parsing turns security metadata into an attacker-controllable string comparison.

## Personal notes

To be added.

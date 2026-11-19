# Cross-Site Request Forgery (CSRF)

Personal notes and PortSwigger Web Security Academy lab progress for Cross-Site Request Forgery. This section explains browser cookie behavior, CSRF defenses, common implementation mistakes, and safe testing principles.

## Learning progress

**10 of 11 labs completed:**

- Apprentice: 1/1 completed
- Practitioner: 9/10 completed
- Overall completion: 10/11
- One intentionally postponed lab: **SameSite Strict bypass via sibling domain**

The postponed lab is not completed or failed. It requires prior understanding of WebSockets and Cross-Site WebSocket Hijacking, and will be revisited during **Advanced Web Security - WebSockets Security** later in Phase 4 of the cybersecurity roadmap.

## Learning objectives

- Understand how CSRF differs from XSS and from authentication or authorization flaws.
- Explain why browsers automatically attach cookies to cross-site requests.
- Evaluate CSRF-token generation, validation, and session binding.
- Understand SameSite cookie modes and their browser-dependent limitations.
- Assess Origin and Referer validation as secondary or supporting controls.
- Apply a repeatable, authorized CSRF testing methodology.
- Recommend root-cause fixes rather than UI-only or request-format-specific defenses.

## Repository structure

- [Lab tracker](./lab-tracker.md)
- [CSRF fundamentals](./csrf-fundamentals.md)
- [CSRF tokens](./csrf-tokens.md)
- [SameSite cookies](./samesite-cookies.md)
- [Referer validation](./referer-validation.md)
- [CSRF testing methodology](./csrf-testing-methodology.md)
- [Lab documentation](./labs/)

## Progress summary

| Difficulty | Completed | Total | Postponed |
|---|---:|---:|---:|
| Apprentice | 1 | 1 | 0 |
| Practitioner | 9 | 10 | 1 |
| **Total** | **10** | **11** | **1** |

The exact titles and statuses are in the [CSRF lab tracker](./lab-tracker.md). Individual lab pages distinguish general technical explanations from verified lab-specific observations.

## Postponed lab

**SameSite Strict bypass via sibling domain**

- Status: POSTPONED
- Reason: Requires prior understanding of WebSockets and Cross-Site WebSocket Hijacking.
- Planned revisit: Advanced Web Security - WebSockets Security, later in Phase 4 of the cybersecurity roadmap.

No solution or exploit details are claimed for this lab.

## References

- [PortSwigger Web Security Academy - Cross-site request forgery](https://portswigger.net/web-security/csrf)
- [OWASP - Cross-Site Request Forgery Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Cross-Site_Request_Forgery_Prevention_Cheat_Sheet.html)
- [MDN - Using HTTP cookies](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/Cookies)
- [MDN - SameSite cookies](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Headers/Set-Cookie)
- [MDN - Origin](https://developer.mozilla.org/en-US/docs/Glossary/Origin)

# Cross-Site Scripting (XSS)

> Personal notes and lab documentation from PortSwigger Web Security Academy.

---

## Project Overview

This repository documents my practical study of Cross-Site Scripting (XSS), including how these flaws are discovered in authorized labs, how payloads reach dangerous sinks, and how secure coding patterns reduce the risk of exploitation.

The focus is on understanding the data flow from attacker-controlled input to browser-executed script, along with the importance of context-aware output handling, browser behavior, and defensive coding.

---

## Learning Objectives

- Understanding Reflected XSS.
- Understanding Stored XSS.
- Understanding DOM-Based XSS.
- Identifying Sources and Sinks.
- Understanding HTML, JavaScript, and URL contexts.
- Understanding encoding and output handling.
- Analyzing filters and basic bypass techniques.
- Understanding XSS impact.
- Understanding XSS prevention and secure coding.
- Developing manual vulnerability discovery skills.

> This repository reflects the current learning stage: I have completed the foundational XSS labs and intentionally postponed six advanced labs until later study.

---

## What is XSS?

Cross-Site Scripting (XSS) is a vulnerability in which attacker-controlled input is included in a web page in a way that causes browser-executed script. The browser treats the injected content as trusted page content, so the malicious script runs under the page's origin.

**Core idea:**

```text
Attacker-controlled input → unsafe rendering or execution → browser executes script
```

---

## XSS Types

| Type | Vulnerability mechanism | Persistence |
|---|---|---|
| Reflected XSS | Input is reflected unsafely in a server response | Usually non-persistent |
| Stored XSS | Input is stored and later rendered unsafely | Persistent |
| DOM-Based XSS | Client-side JavaScript processes untrusted data through an unsafe operation | Can be reflected or stored |

These categories describe different aspects of XSS and can overlap. DOM-Based XSS is defined by unsafe client-side processing of data; it may involve reflected or stored attacker-controlled values.

---

## Key Concepts

### Source
A source is a place where untrusted data enters the application.

```text
location.search    → query string (?q=...)
location.hash      → fragment (#...)
location.href      → full URL
location.pathname  → path portion of URL
document.referrer  → referring URL
window.name        → browser-managed value that may be attacker-influenced in some scenarios
```

A source only matters when the data can be influenced by an attacker under the relevant threat model.

### Sink
A sink is a place where data is used or interpreted in a way that can trigger browser behavior.

```text
innerHTML             → renders HTML
element.outerHTML     → writes HTML
document.write()      → writes to DOM
eval()                → executes JS
jQuery DOM methods    → manipulate DOM using HTML or selectors
javascript: URL values → browser executes when activated
```

**The vulnerability exists when:**

```text
Attacker-controlled input → source → data processing → sink → browser interpretation
```

This flow is not automatically exploitable, but it is the core pattern to analyze.

---

## Repository Navigation

- [lab-tracker.md](./lab-tracker.md)
- [methodology/discovery-workflow.md](./methodology/discovery-workflow.md)
- [reflected-xss/notes.md](./reflected-xss/notes.md)
- [stored-xss/notes.md](./stored-xss/notes.md)
- [dom-xss/notes.md](./dom-xss/notes.md)
- [dom-xss/document-write.md](./dom-xss/document-write.md)
- [dom-xss/innerHTML.md](./dom-xss/innerHTML.md)
- [dom-xss/jquery.md](./dom-xss/jquery.md)
- [dom-xss/angularjs.md](./dom-xss/angularjs.md)
- [contexts/notes.md](./contexts/notes.md)
- [bypasses-and-csp/notes.md](./bypasses-and-csp/notes.md)
- [exploitation/notes.md](./exploitation/notes.md)

---

## Progress Overview

### Completed labs (24)

#### Reflected XSS
- Reflected XSS into HTML context with nothing encoded.

#### Stored XSS
- Stored XSS into HTML context with nothing encoded.
- Stored XSS into anchor href attribute with double quotes HTML-encoded.

#### DOM-based XSS
- DOM XSS in document.write sink using source location.search.
- DOM XSS in innerHTML sink using source location.search.
- DOM XSS in jQuery anchor href attribute sink using location.search source.
- DOM XSS in jQuery selector sink using a hashchange event.
- DOM XSS in document.write sink using location.search inside a select element.
- DOM XSS in AngularJS expression with angle brackets and double quotes HTML-encoded.
- Reflected DOM XSS.
- Stored DOM XSS.

#### XSS contexts
- Reflected XSS into attribute with angle brackets HTML-encoded.
- Reflected XSS into a JavaScript string with angle brackets HTML encoded.
- Reflected XSS into a JavaScript string with single quote and backslash escaped.
- Reflected XSS into a JavaScript string with angle brackets and double quotes HTML encoded and single quotes escaped.
- Stored XSS into onclick event with angle brackets and double quotes HTML encoded and single quotes and backslash escaped.
- Reflected XSS into a template literal with angle brackets, single quotes, double quotes, backslash and backticks Unicode-escaped.
- Reflected XSS into HTML context with most tags and attributes blocked.
- Reflected XSS into HTML context with all tags blocked except custom ones.
- Reflected XSS with some SVG markup allowed.
- Reflected XSS in canonical link tag.

#### XSS exploitation
- Exploiting cross-site scripting to steal cookies.
- Exploiting cross-site scripting to capture passwords.
- Exploiting XSS to bypass CSRF defenses.

### Postponed labs (6)

These labs are intentionally postponed and not marked as failed. They require prerequisites such as AngularJS internals, sandboxing, CSP analysis, browser behavior, and advanced filter analysis.

#### Expert
- Reflected XSS with AngularJS sandbox escape without strings.
- Reflected XSS with AngularJS sandbox escape and CSP.
- Reflected XSS with event handlers and href attributes blocked.
- Reflected XSS in a JavaScript URL with some characters blocked.
- Reflected XSS protected by CSP, with CSP bypass.

#### Practitioner
- Reflected XSS protected by very strict CSP, with dangling markup attack.

These topics will be revisited after studying the necessary browser and security concepts.

---

## Where to Start

The foundational XSS learning stage is complete: 24 PortSwigger XSS labs are completed and 6 advanced labs are intentionally postponed. SQL Injection is also complete, including all PortSwigger labs. The next recommended topic is **Authentication Vulnerabilities**; there is no need to repeat beginner XSS or SQL Injection material.

Focus areas:
- Authentication mechanisms and authentication vs. authorization.
- Password-based authentication weaknesses.
- Brute-force protection, rate limiting, and username enumeration.
- Password reset vulnerabilities and multi-factor authentication weaknesses.
- Session management and related security issues.

Keep this XSS documentation as a reference while moving through other OWASP Web Security vulnerabilities. Revisit the six postponed XSS labs later, after strengthening JavaScript, browser security, AngularJS, and CSP knowledge.

---

## Current State

This repository reflects the current stage of the learning journey: 24 PortSwigger XSS labs have been completed, 6 advanced labs are intentionally postponed, and the focus is now moving toward other OWASP web-vulnerability topics while continuing to deepen JavaScript, browser security, CSP, and AngularJS knowledge.

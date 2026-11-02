# Lab Tracker

This file records the current status of the XSS labs in this repository.

## Completed

### Reflected XSS
- Reflected XSS into HTML context with nothing encoded.

### Stored XSS
- Stored XSS into HTML context with nothing encoded.
- Stored XSS into anchor href attribute with double quotes HTML-encoded.

### DOM-based XSS
- DOM XSS in document.write sink using source location.search.
- DOM XSS in innerHTML sink using source location.search.
- DOM XSS in jQuery anchor href attribute sink using location.search source.
- DOM XSS in jQuery selector sink using a hashchange event.
- DOM XSS in document.write sink using location.search inside a select element.
- DOM XSS in AngularJS expression with angle brackets and double quotes HTML-encoded.
- Reflected DOM XSS.
- Stored DOM XSS.

### XSS contexts
- Reflected XSS into attribute with angle brackets HTML-encoded.
- Reflected XSS into a JavaScript string with angle brackets HTML-encoded.
- Reflected XSS into a JavaScript string with single quote and backslash escaped.
- Reflected XSS into a JavaScript string with angle brackets and double quotes HTML-encoded and single quotes escaped.
- Stored XSS into onclick event with angle brackets and double quotes HTML-encoded and single quotes and backslash escaped.
- Reflected XSS into a template literal with angle brackets, single quotes, double quotes, backslash and backticks Unicode-escaped.
- Reflected XSS into HTML context with most tags and attributes blocked.
- Reflected XSS into HTML context with all tags blocked except custom ones.
- Reflected XSS with some SVG markup allowed.
- Reflected XSS in canonical link tag.

### XSS exploitation
- Exploiting cross-site scripting to steal cookies.
- Exploiting cross-site scripting to capture passwords.
- Exploiting XSS to bypass CSRF defenses.

Total completed: 24 labs.

---

## Postponed

These labs are intentionally postponed rather than failed. They require additional study of AngularJS, sandbox escapes, CSP, browser parsing, and advanced filter analysis.

### Expert
- Reflected XSS with AngularJS sandbox escape without strings.
- Reflected XSS with AngularJS sandbox escape and CSP.
- Reflected XSS with event handlers and href attributes blocked.
- Reflected XSS in a JavaScript URL with some characters blocked.
- Reflected XSS protected by CSP, with CSP bypass.

### Practitioner
- Reflected XSS protected by very strict CSP, with dangling markup attack.

Total postponed: 6 labs.

---

## Notes

- This tracker reflects the current learning status and deliberately avoids claiming mastery beyond the labs that are explicitly recorded as completed.
- Postponed labs will be revisited after gaining the necessary knowledge in JavaScript, browser internals, and modern defense mechanisms.

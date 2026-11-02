# XSS Bypasses & CSP

This section separates what has already been studied from the more advanced browser-security topics that are intentionally postponed.

---

## Completed filter and markup restriction labs

The following labs are completed and should be treated as part of the foundational XSS learning stage:

- Reflected XSS into HTML context with most tags and attributes blocked.
- Reflected XSS into HTML context with all tags blocked except custom ones.
- Reflected XSS with some SVG markup allowed.
- Reflected XSS in canonical link tag.

These labs illustrate the importance of understanding the browser's parsing rules and the ways in which filter restrictions may still leave exploitable paths.

---

## WAF & Filter Bypass

WAFs and input filters try to block XSS payloads by restricting tags, attributes, or patterns. Bypassing them requires understanding exactly what the filter is checking and then finding an alternative payload or injection path that the browser still interprets as active content.

**Common techniques include:**
- alternative HTML or SVG elements
- custom elements or unusual markup
- case manipulation and normalization problems
- encoding tricks that bypass naive validation

---

## CSP concepts

Content Security Policy (CSP) restricts which scripts, styles, and other active resources a browser may load. A strong CSP can reduce the impact of an XSS issue by limiting script execution.

**Example:**
```http
Content-Security-Policy: script-src 'self'
```

This blocks script execution from untrusted sources unless explicitly allowed.

CSP is important, but it is not a universal solution. It must be combined with correct input handling, safe output encoding, and careful review of the application architecture.

---

## Advanced CSP and sandbox topics (postponed)

The following labs remain intentionally postponed and are not marked as failed:

### Expert labs
- Reflected XSS with AngularJS sandbox escape without strings.
- Reflected XSS with AngularJS sandbox escape and CSP.
- Reflected XSS with event handlers and href attributes blocked.
- Reflected XSS in a JavaScript URL with some characters blocked.
- Reflected XSS protected by CSP, with CSP bypass.

### Practitioner lab
- Reflected XSS protected by very strict CSP, with dangling markup attack.

These topics require deeper study of JavaScript, browser security, CSP enforcement, and advanced filter/parser behavior.

---

## Key Takeaways

- Completed filter-restriction labs are part of the current skill set.
- Advanced CSP and sandbox bypass work remains a future learning stage.
- The correct mindset is to reason about what the browser will interpret, not just what a filter is trying to block.

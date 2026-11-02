# XSS Contexts

> This section documents the key output contexts involved in XSS. The goal is to understand that one payload is not valid in every context and that encoding must match the browser interpretation.

---

## What Is an Injection Context?

The context is the exact place where attacker-controlled data is inserted into HTML, JavaScript, URLs, or another browser-interpreted format. The same payload may work in one context and fail in another because the browser parses the surrounding syntax differently.

---

## Context 1: HTML Element Context

Your input lands directly in the HTML body.

```html
<p>Hello, [YOUR INPUT]</p>
```

**If no encoding is applied, basic payloads may work:**
```html
<script>alert(1)</script>
```

**Completed lab examples:**
- Reflected XSS into HTML context with nothing encoded.
- Stored XSS into HTML context with nothing encoded.

---

## Context 2: HTML Attribute Context

Your input lands inside an HTML attribute.

```html
<input value="[YOUR INPUT]">
```

If the application HTML-encodes angle brackets, a direct `<script>` payload may fail. However, if the attribute can be broken out of, a payload such as an event handler may still execute.

```html
<input value="" onmouseover="alert(1)">
```

**Completed labs:**
- Reflected XSS into attribute with angle brackets HTML-encoded.
- Stored XSS into anchor href attribute with double quotes HTML-encoded.

---

## Context 3: JavaScript String Context

Your input lands inside a JavaScript string.

```html
<script>
    var search = '[YOUR INPUT]';
</script>
```

The goal is to break out of the quote-delimited string and inject executable JavaScript.

```javascript
var search = ''; alert(1); var x='';
```

**Completed labs:**
- Reflected XSS into a JavaScript string with angle brackets HTML encoded.
- Reflected XSS into a JavaScript string with single quote and backslash escaped.
- Reflected XSS into a JavaScript string with angle brackets and double quotes HTML encoded and single quotes escaped.

---

## Context 4: JavaScript Template Literal Context

Your input lands inside a JavaScript template literal.

```javascript
var message = `Hello, [YOUR INPUT]`;
```

Template literals can evaluate `${...}` expressions when the input is interpreted in that context.

```javascript
var message = `Hello, ${alert(1)}`;
```

**Completed lab:**
- Reflected XSS into a template literal with angle brackets, single quotes, double quotes, backslash and backticks Unicode-escaped.

---

## Context 5: Event-Handler Attribute Context

Your input is placed in an event handler such as `onerror`, `onclick`, or `onmouseover`.

```html
<img src=x onerror=alert(1)>
```

**Completed lab:**
- Stored XSS into onclick event with angle brackets and double quotes HTML encoded and single quotes and backslash escaped.

---

## Context 6: URL-Related Context

Your input lands in a URL or attribute that is interpreted as a navigable URL.

```html
<a href="javascript:alert(1)">Click</a>
```

**Completed labs:**
- Stored XSS into anchor href attribute with double quotes HTML-encoded.
- Reflected XSS in canonical link tag.

---

## Summary Table

| Context | Typical break condition | Example payload pattern |
|---|---|---|
| HTML element | raw HTML is allowed | `<script>alert(1)</script>` |
| HTML attribute | quote or attribute breakout | `" onmouseover="alert(1)` |
| JavaScript string | quote or string termination | `';alert(1);//` |
| Template literal | `${...}` evaluation | `${alert(1)}` |
| Event handler | event attribute parsing | `onerror=alert(1)` |
| URL context | dangerous scheme or navigation | `javascript:alert(1)` |

---

## Why Context Matters

Encoding must match the output context. HTML encoding does not protect JavaScript string contexts, JavaScript escaping does not protect HTML contexts, and URL encoding is not a universal XSS defense.

The general rule is:

- HTML output requires HTML-safe encoding.
- Attribute output requires attribute-safe encoding.
- JavaScript string output requires JavaScript string escaping.
- URL output requires URL-aware handling.
- Template contexts must be treated as code execution contexts.

A strong defense is to use the correct escaping or sanitization for the exact output context, rather than relying on a single encoding scheme everywhere.

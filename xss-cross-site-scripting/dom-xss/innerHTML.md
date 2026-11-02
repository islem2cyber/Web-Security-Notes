# DOM XSS — innerHTML Sink

## The Sink

`innerHTML` sets the HTML content of an element. It parses the assigned string as HTML and inserts the resulting DOM nodes, which makes it a dangerous sink when attacker-controlled data reaches it.

```javascript
element.innerHTML = userInput;
```

This is dangerous because the browser will parse markup in the assigned value, not just treat it as plain text.

> Modern browsers usually do not execute `<script>` tags inserted via `innerHTML`, but event handlers such as `onerror` and `onload` can still execute when the markup is parsed as HTML.

---

## Lab: DOM XSS in innerHTML using location.search

**Level:** Apprentice ✅ Solved

**Vulnerable code:**
```javascript
document.getElementById('searchMessage').innerHTML = location.search;
```

**What happens normally:**
```text
URL: ?search=hello
→ <div id="searchMessage">?search=hello</div>
```

**The attack:**
```text
Payload: <img src=1 onerror=alert(1)>
```

**Full URL:**
```text
https://site.com/?search=<img src=1 onerror=alert(1)>
```

**Result in DOM:**
```html
<div id="searchMessage">
  <img src=1 onerror=alert(1)>
</div>
```

The browser attempts to load the image, the load fails, and the `onerror` handler executes.

---

## innerHTML vs textContent

| Property | Behavior | Security note |
|---|---|---|
| `innerHTML` | Parses the assigned value as HTML | Dangerous when untrusted HTML reaches it |
| `textContent` | Inserts plain text without parsing HTML | Usually the correct choice for untrusted plain text |
| `innerText` | Exposes rendered text from the DOM | Not the general security replacement for `innerHTML` |

`textContent` is typically the appropriate choice when the goal is to display untrusted text. It is not a universal solution for every browser context: URL handling, JavaScript execution, CSS, and HTML parsing each require context-specific controls.

---

## Key Takeaways

- `innerHTML` is a common DOM XSS sink because it parses the assigned value as HTML.
- Use `textContent` for untrusted plain text instead of inserting raw HTML.
- `innerText` is not the general security replacement for `innerHTML`.
- The correct defense depends on the actual context: HTML, attribute, URL, JavaScript, and CSS must each be handled appropriately.

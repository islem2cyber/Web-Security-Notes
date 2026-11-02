# Reflected XSS

## How It Works

Reflected XSS occurs when attacker-controlled input is included in an HTTP request and then reflected back in a response without safe output handling. If the browser interprets the returned content as executable HTML, script, or another active context, the payload can execute.

```text
Attacker-controlled request
        ↓
Application reflects input in response
        ↓
Browser interprets the resulting content
        ↓
Script executes under the page's origin
```

> Reflected XSS is usually non-persistent because the payload is delivered in the request itself. The victim must load the malicious request, but delivery can happen through different mechanisms, including a crafted URL or a maliciously generated request.

---

## Basic Example

**Vulnerable code (server-side):**
```text
Search results for: <user input here>
```

**Normal request:**
```text
https://site.com/search?q=hello
→ Search results for: hello
```

**Malicious request:**
```text
https://site.com/search?q=<script>alert(1)</script>
→ Search results for: <script>alert(1)</script>
```

The browser sees the reflected `<script>` element and executes it.

---

## Lab: Reflected XSS into HTML context with nothing encoded

**Level:** Apprentice ✅ Solved

**Scenario:** Search box reflects input directly into the HTML response with no encoding.

**Payload:**
```html
<script>alert(1)</script>
```

**Why it works:** The input is inserted into HTML without safe output handling, so it is interpreted as active markup by the browser.

---

## Key Takeaways

- Always check where the attacker-controlled value appears in the response.
- If the value lands in HTML without context-appropriate encoding, payloads such as `<script>` may execute.
- Reflection does not guarantee exploitability; the result depends on the output context, browser behavior, and relevant security controls.
- Reflected XSS is often found in search parameters, error messages, and other request-driven responses.

# Stored XSS

## How It Works

Stored XSS occurs when attacker-controlled input is saved by the application and later retrieved and rendered to other users. The malicious payload is not delivered in a single request; it becomes part of the application's stored content.

```text
Attacker submits malicious input
        ↓
Application stores it
        ↓
Another user loads the affected page
        ↓
The stored value is rendered and may execute in the browser
```

> Stored XSS is persistent because the payload is saved and later rendered. This can affect multiple users and broaden impact, but severity still depends on the affected feature, user exposure, privileges, and the execution context.

---

## Basic Example

**Vulnerable feature:** Blog comments

**Attacker submits:**
```html
<script>alert(1)</script>
```

**What gets saved in the database:**
```text
<script>alert(1)</script>
```

**What other users see when the page loads:**
The browser may interpret the stored HTML and execute the script.

---

## Lab: Stored XSS into HTML context with nothing encoded

**Level:** Apprentice ✅ Solved

**Scenario:** Comment section stores and renders input with no sanitization.

**Payload:**
```html
<script>alert(1)</script>
```

**Why it works:** The application stores the malicious input and later renders it as HTML without safe output handling.

---

## Stored vs Reflected

| | Reflected | Stored |
|---|---|---|
| Payload location | Request/response | Stored application data |
| Persistence | Usually no | Yes |
| Who is affected | Victim who loads the crafted request | Any user who views the affected content |
| Delivery | Request-driven | Stored and later retrieved |
| Impact | Depends on context and exposure | Can affect multiple users |

---

## Key Takeaways

- Stored XSS is often more impactful because it can affect many users.
- Common injection points include comments, profile fields, product reviews, ticket content, and user-generated data.
- Severity depends on the actual feature and execution context, not on persistence alone.
- Even if input is sanitized on display, the same value may still be dangerous if it is stored without proper validation or encoding.

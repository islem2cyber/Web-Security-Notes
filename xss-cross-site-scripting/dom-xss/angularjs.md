# DOM XSS — AngularJS Expression Injection

## What is AngularJS?

AngularJS is a JavaScript framework that processes HTML pages and adds dynamic behavior. When it sees an `ng-app` attribute, AngularJS takes control of that DOM region and evaluates AngularJS expressions, which are not the same thing as general JavaScript execution in every context.

```html
<div ng-app>
    {{ 2 + 2 }}
</div>
```

**Result rendered in browser:** `4`

---

## The Vulnerability

If attacker-controlled data is reflected into an AngularJS-controlled template expression, AngularJS may evaluate that expression according to AngularJS expression rules. This can happen even when standard HTML payloads like `<script>` are blocked or HTML-encoded.

```html
<body ng-app>
    Search results for: {{ userInput }}
</body>
```

This is a form of expression injection, not simply “ordinary JavaScript execution everywhere inside `{{ }}`.”

---

## Why angle brackets do not save the page

A normal HTML-based XSS payload may rely on `<script>`, `<img>`, or event-handler markup. If the application HTML-encodes angle brackets, those payloads may fail.

AngularJS expression injection changes that because the dangerous primitive is the AngularJS expression itself, not a standard HTML tag. The page is vulnerable when the framework evaluates untrusted content in a template context.

---

## Lab: DOM XSS in AngularJS expression

**Level:** Practitioner ✅ Solved

**Setup:** The page uses `ng-app`. User input is reflected into an AngularJS-controlled expression context.

**Payload:**
```text
{{constructor.constructor('alert(1)')()}}
```

**How it works:**
```javascript
constructor
.constructor
('alert(1)')
()
```

This payload reaches the `Function` constructor and calls it dynamically, which can trigger script execution.

---

## Important distinctions

### 1. AngularJS expression language
AngularJS expressions are evaluated using AngularJS expression rules, not the same as arbitrary browser-executed JavaScript in every template or HTML context.

### 2. AngularJS expression injection
This is the issue where untrusted input is reflected into an expression context and evaluated by AngularJS.

### 3. Client-Side Template Injection (CSTI)
This is a broader category describing template evaluation vulnerabilities in client-side frameworks or template engines.

### 4. AngularJS sandbox escape
A sandbox escape is a more advanced and more specific issue involving AngularJS sandbox restrictions and how expression evaluation may be bypassed or abused.

These concepts are related but not interchangeable. A vulnerable template context is not automatically proof of a sandbox escape, and payload behavior can vary across AngularJS versions and configuration.

---

## Advanced AngularJS notes

The more advanced AngularJS studies concern sandbox escapes, CSP, and expression restrictions. These are deliberately postponed until the relevant browser-security and AngularJS knowledge is strengthened.

### Sandbox escape
AngularJS sandboxing restricts what expressions can access. Sandbox escapes are a separate class of issue requiring deeper understanding of framework internals and browser behavior.

### CSP interaction
CSP can limit script execution, but a bypass depends on the exact browser behavior, policy, and template evaluation path involved.

---

## Key Takeaways

- AngularJS expressions are evaluated in a template context, not treated as ordinary HTML text.
- Expression injection can bypass simple HTML encoding protections if the expression mechanism is reached.
- A vulnerable template context is not the same as a sandbox escape.
- The exact behavior depends on AngularJS version, framework configuration, and browser context.

---

## 🚧 Advanced AngularJS (Postponed)

The following labs remain postponed because they require a deeper understanding of AngularJS internals, sandbox behavior, and CSP analysis:

- Reflected XSS with AngularJS sandbox escape without strings.
- Reflected XSS with AngularJS sandbox escape and CSP.

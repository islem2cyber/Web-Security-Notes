# Null Byte Injection

A null byte is a byte value representing the end of a C-style string in older APIs or implementations. It is commonly represented in text as `%00` after URL encoding.

## Older implementation behavior

In some older native APIs or legacy application bridges, a string containing a null byte could be truncated when converted to a C string. An application that checks the full supplied string but passes it to a vulnerable API might validate an extension or suffix that is never used for the final native file access.

For example, an extension check could inspect the text before `%00`, while an older native string conversion could terminate at the null byte and remove text after it. The result could differ from the application's intended file name.

This is an implementation-specific explanation and is not a universal bypass against modern runtimes.

## Modern runtime behavior

Modern languages and libraries commonly reject embedded null characters or return an error. Python, for example, may reject embedded null bytes in filesystem calls rather than silently truncating them. Native libraries and frameworks vary, so application behavior must be verified in the specific environment.

The fact that modern APIs reject `%00` does not make null-byte handling irrelevant to legacy applications or to bridges with unsafe native conversions.

## Lab distinction

The associated PortSwigger lab demonstrates a specific implementation path in which file-extension validation is bypassed under the lab environment's behavior. It is not proof that all modern systems accept null bytes or universally truncate strings.

No exact payload or application behavior is claimed here because the lab-specific request details were not supplied.

## Defensive guidance

- Reject control characters, including null bytes, when they are not legitimate user input.
- Do not rely on string termination as a security boundary.
- Use modern filesystem APIs and verify their behavior in the installed runtime.
- Use server-side identifiers or allowlists rather than extension validation.
- Resolve and validate the final canonical path.
- Test legacy dependencies and native integrations separately.

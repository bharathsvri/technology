# Security — XSS, Sanitization, and `DomSanitizer`

## Angular sanitization

By default Angular **sanitizes** interpolated values and bound properties in templates to reduce **XSS**.

## `bypassSecurityTrustHtml` (dangerous)

Only use when you **trust** the HTML source completely—prefer **Markdown → safe subset** pipelines or server-side rendering with strict CSP.

## Content Security Policy (CSP)

Combine Angular’s defenses with headers: `default-src 'self'`, `script-src` with nonces/hashes for inline scripts if needed.

## `HttpClient` JSON

Do not `eval` JSON; validate payloads with **schemas** (zod, class-validator on server).

## Open redirects

Validate `returnUrl` query params server-side and in router guards.

Next: [HTTP uploads and progress](36-HTTP-Upload-Download-and-Progress.md).

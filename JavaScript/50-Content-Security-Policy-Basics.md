# Content Security Policy (CSP) Basics

**CSP** is an HTTP header (`Content-Security-Policy`) that **restricts** where scripts, styles, images, and connections may load from—**mitigating XSS** impact even when a bug injects HTML.

---

## Common directives

- **`default-src`**: fallback for other fetch directives.
- **`script-src`**: allowlist script origins; **`'unsafe-inline'`** weakens protection.
- **`frame-ancestors`**: clickjacking control (replaces **`X-Frame-Options`** in modern setups).

---

## Nonces / hashes

**`script-src 'nonce-...'`** allows **specific** inline scripts generated per request—better than **`unsafe-inline`**.

---

## Report-Only mode

**`Content-Security-Policy-Report-Only`** logs violations without blocking—use to **roll out** safely.

---

## Related

- [Fetch API and HTTP](21-Fetch-API-and-HTTP.md)
- [Secure Coding OWASP Java Focus](../Java%20Concepts/111_Secure_Coding_OWASP_Java_Focus.md)

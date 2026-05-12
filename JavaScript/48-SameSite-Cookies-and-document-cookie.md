# `SameSite` Cookies and `document.cookie`

**`SameSite`** (`Strict`, `Lax`, `None`) controls whether cookies are sent on **cross-site** requests—critical for **CSRF** and **OAuth** flows. **`document.cookie`** remains the legacy **string** API; prefer **server `Set-Cookie`** for HttpOnly flags.

---

## `None` requires `Secure`

Modern browsers require **`Secure`** when **`SameSite=None`**.

---

## `HttpOnly`

Not accessible from **`document.cookie`**—mitigates **XSS** token theft for **session** cookies.

---

## Interview soundbite

“**Lax** is default in many browsers—**cross-site POST** won’t carry cookies, affecting **3DS** / **payment** callbacks—plan **redirect** flows.”

---

## Related

- [Fetch API and HTTP](21-Fetch-API-and-HTTP.md)
- [Spring Boot CORS CSRF and Browser Security](../Spring%20Boot%20Concepts/78-Spring-Boot-CORS-CSRF-and-Browser-Security.md)

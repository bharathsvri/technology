# CORS, CSRF, and Browser Security for REST APIs

Browser-facing **Spring Boot** APIs must set **CORS** deliberately and understand when **CSRF** protections apply (cookies vs tokens).

---

## CORS (Cross-Origin Resource Sharing)

- **Browser** enforces CORS for **XHR/fetch** from a **different origin** (scheme+host+port).
- Server responds with **`Access-Control-Allow-Origin`** (specific origin vs `*` with constraints), **`Allow-Methods`**, **`Allow-Headers`**, optional **`Allow-Credentials`**.

**Spring**: `WebMvcConfigurer.addCorsMappings` or **`@CrossOrigin`** on controllers (central config preferred in prod).

**Interview pitfall**: CORS is **not** an authorization system—it only relaxes **browser** reads; **curl/Postman** ignore CORS.

---

## CSRF

- Relevant when the browser **automatically sends cookies** (session auth) on **mutating** requests.
- **JWT in `Authorization` header** from SPA is often **CSRF-immune** (browser does not attach bearer automatically cross-site).

**Spring Security**: CSRF token for session cookie flows; **disable** only with clear threat model (pure stateless bearer APIs).

---

## SameSite cookies

`SameSite=Lax/Strict` reduces **cross-site** cookie abuse; understand **OAuth** redirect flows that need **`None; Secure`**.

---

## Related

- [Spring Boot Security](12-Spring-Boot-Security.md)
- [Spring Boot OAuth2 and Spring Security](57-Spring-Boot-OAuth2-and-Spring-Security.md)

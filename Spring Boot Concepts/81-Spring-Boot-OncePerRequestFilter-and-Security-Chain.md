# `OncePerRequestFilter` and the Security Filter Chain

Spring Security is implemented as a **chain** of **`Filter`** beans. **`OncePerRequestFilter`** guarantees **single execution** per request even when the dispatcher **forwards** or **includes** (avoiding double auth/logging).

---

## Why it matters

- **Forwarded** error pages or **internal forwards** can re-enter filters.
- Security decisions (CSRF, session fixation) must be **consistent** per logical request.

---

## Custom filter placement

Use **`SecurityFilterChain`** DSL **`addFilterBefore` / `addFilterAfter`** with explicit **`Order`** relative to **`UsernamePasswordAuthenticationFilter`**, etc.

**Pitfall**: wrong order → **anonymous** principal still visible downstream or **CORS** preflight mishandled.

---

## `shouldNotFilter`

Override to **skip** filter for static assets or **`OPTIONS`** requests when appropriate.

---

## Related

- [Spring Boot Security](12-Spring-Boot-Security.md)
- [Spring Boot Filters Interceptors](34-Spring-Boot-Filters-Interceptors.md)
- [HandlerInterceptor vs Servlet Filter](../Spring/34-HandlerInterceptor-vs-Servlet-Filter.md)

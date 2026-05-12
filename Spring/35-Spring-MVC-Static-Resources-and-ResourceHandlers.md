# Static Resources and `ResourceHttpRequestHandler`

Spring MVC serves **classpath** and **filesystem** static assets via **`ResourceHttpRequestHandler`** (configured by **`ResourceHandlerRegistry`** in Java config or Boot conventions).

---

## Boot defaults

`spring.web.resources` maps **`/static`**, **`public`**, etc., to **`/**`** with **cache** headers—override for **immutable** fingerprinted assets.

---

## `ResourceResolver` / `ResourceTransformer`

**Versioned URLs**, **Gzip/Brotli** variants, and **content-based hashes** plug in here for advanced CDN patterns.

---

## Security

- **Do not** expose sensitive directories via broad **`file:`** resource locations.
- **Path traversal** mitigations live in handler chain—validate custom resolvers.

---

## Related

- [View Resolution](13-View-Resolution.md)
- [Spring Boot Resource Handling](../Spring%20Boot%20Concepts/43-Spring-Boot-Resource-Handling.md)

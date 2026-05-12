# `HandlerInterceptor` vs Servlet `Filter`

Both sit on the **request path**, but **different lifecycles and responsibilities**—Spring MVC interviews often contrast them.

---

## Servlet `Filter`

- Part of the **Servlet container** chain—runs **before** **`DispatcherServlet`** (and can wrap **non-Spring** servlets if configured).
- Good for **cross-cutting** concerns: **gzip**, **CORS** (lower layers), **request IDs**, **authentication** decoding at the **wire** level.

---

## `HandlerInterceptor`

- Spring MVC concept—runs **around handler execution** with access to **`HandlerMethod`**, model/view, **afterCompletion** even if errors occur (with exceptions parameter).
- Good for **authorization tied to MVC mappings**, **locale** resolution, **timing** MVC handlers.

---

## Ordering

- **`OncePerRequestFilter`** avoids duplicate execution in forwards/includes.
- **`order()`** on **`WebMvcConfigurer.addInterceptors`** vs **`FilterRegistrationBean`** `@Order`.

---

## Related

- [DispatcherServlet and Request Handling](10-DispatcherServlet-and-Request-Handling.md)
- [Spring Boot Filters Interceptors](../Spring%20Boot%20Concepts/34-Spring-Boot-Filters-Interceptors.md)

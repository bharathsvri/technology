# DispatcherServlet and Request Handling

The **`DispatcherServlet`** is a standard **`HttpServlet`** subclass registered in **`web.xml`** or via **`ServletRegistrationBean`** (Boot). It is the **single entry point** for Spring MVC for its mapped URL pattern.

---

## Boot vs traditional WAR

**Traditional**: `web.xml` declares servlet + `contextConfigLocation`.

**Spring Boot**: `SpringBootServletInitializer` for WAR; executable JAR uses embedded Tomcat and auto-registers `DispatcherServlet` with sensible defaults (`spring.mvc.servlet.path`).

---

## Request processing pipeline (detail)

1. **`DispatcherServlet#doDispatch`** — entry.
2. **`multipart`** resolution if `multipart/form-data`.
3. **Locale** resolution.
4. **Handler lookup** — first non-null `HandlerMapping` wins (order matters).
5. **Interceptors** — `preHandle` chain; if any returns `false`, abort.
6. **Handler execution** — argument resolvers build method parameters (`@RequestBody`, `@PathVariable`, …).
7. **Exception handling** — `@ExceptionHandler` from `@ControllerAdvice` if thrown.
8. **Return value handling** — e.g. write JSON or resolve view.
9. **Interceptors** — `postHandle`, then `afterCompletion` (even after exception, with rules).

---

## Async request processing

Return **`Callable<T>`** or **`DeferredResult<T>`** to release the servlet thread while work continues on another thread—important for **long-polling** or heavy I/O when configured with **`asyncSupported`**.

---

## Multipart configuration

Boot: `spring.servlet.multipart.max-file-size`, `max-request-size`. Spring resolves **`MultipartFile`** parameters from the request.

---

## Character encoding

Use **`CharacterEncodingFilter`** or server default UTF-8; mismatched encoding corrupts JSON/form data.

---

## Common configuration knobs (Boot properties)

- `spring.mvc.servlet.path` — servlet mapping.
- `spring.mvc.throw-exception-if-no-handler-found` — surface 404 as exception to handlers.
- `spring.web.resources.*` — static resource locations.

---

## Next

- [Controllers and RequestMapping](11-Controllers-and-RequestMapping.md)

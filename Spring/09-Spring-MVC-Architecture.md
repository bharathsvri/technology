# Spring MVC Architecture

**Spring MVC** implements the **Model–View–Controller** pattern for servlet environments. The **`DispatcherServlet`** is the **front controller**: every HTTP request hits it first, then Spring dispatches to your **controller** methods and resolves **views** or **response bodies**.

---

## Big picture flow

1. **Client** sends HTTP request to servlet container (Tomcat, etc.).
2. **`DispatcherServlet`** receives the request (mapped e.g. to `/` or `/app/*`).
3. **`HandlerMapping`** selects a **handler** (usually a `@RequestMapping` method).
4. **`HandlerAdapter`** invokes the handler (annotation resolution, argument resolvers).
5. Return value is processed by **`HandlerMethodReturnValueHandler`**:
   - **`@ResponseBody` / `ResponseEntity`** → `HttpMessageConverter` writes JSON/XML.
   - **`String` / `ModelAndView`** → **`ViewResolver`** resolves a view template.
6. **`View`** renders HTML (or other) to response.

---

## Key interfaces (mental map)

| Component | Role |
|-----------|------|
| **`DispatcherServlet`** | Central servlet, orchestrates pipeline |
| **`HandlerMapping`** | URL + method + consumes/produces → handler |
| **`HandlerAdapter`** | Invokes different handler types uniformly |
| **`HandlerInterceptor`** | `preHandle` / `postHandle` / `afterCompletion` |
| **`LocaleResolver` / `ThemeResolver`** | i18n / theming (less common in pure REST APIs) |
| **`MultipartResolver`** | File upload parsing |

---

## DispatcherServlet and multiple servlets

Legacy apps might map different `DispatcherServlet` instances to `/api/*` vs `/*` with separate `WebApplicationContext` children. **Boot** typically uses **one** `DispatcherServlet` mapped to `/` (or `server.servlet.context-path`).

---

## REST vs server-rendered MVC

| Style | Typical return | View resolvers? |
|-------|----------------|-----------------|
| **REST** | `@RestController`, DTOs, `ResponseEntity` | No (message converters) |
| **SSR MVC** | Thymeleaf/JSP view name | Yes |

Same `DispatcherServlet` pipeline; different return value handling.

---

## Why this matters for debugging

- **404** → no matching `HandlerMapping` (wrong path, missing `@RequestMapping`, wrong HTTP method).
- **406** → content negotiation / no suitable `HttpMessageConverter`.
- **415** → unsupported `Content-Type` on request body.

---

## Next

- [DispatcherServlet and Request Handling](10-DispatcherServlet-and-Request-Handling.md)

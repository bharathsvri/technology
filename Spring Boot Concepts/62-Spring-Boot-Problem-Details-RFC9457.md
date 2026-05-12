# Spring Boot: Problem Details for HTTP APIs (RFC 9457)

## What Changed?
**RFC 9457** (successor to RFC 7807) standardizes **Problem Details** for HTTP APIs: a small JSON document describing **what went wrong** in a machine-readable way (`type`, `title`, `status`, `detail`, `instance`, optional extensions).

Spring MVC / WebFlux expose this via **`ProblemDetail`** and related support.

## Enable Problem Details
In Spring Boot 3.x:

```yaml
spring:
  mvc:
    problemdetails:
      enabled: true
```

(WebFlux has analogous properties under `spring.webflux`.)

## Typical Controller Advice Pattern
Map exceptions to `ProblemDetail` with appropriate **HTTP status**, **title**, and **detail** (safe for clients—no stack traces in production bodies).

Design guidelines:
- Use a stable **`type`** URI (often documentation URLs) per error category.
- Put **correlation IDs** in `instance` or an extension field.
- Align with your **OpenAPI** document so clients know extension schemas.

## Relationship to `ResponseEntity`
`ProblemDetail` can be returned as the body of `ResponseEntity` while still setting headers (e.g., `Retry-After` for throttling).

## Why Not Only `{ "message": "Error" }`?
Uniform fields improve **client libraries**, **API gateways**, and **observability** rules. Problem Details also play well with **content negotiation** when enabled alongside JSON error bodies.

## Related Topics
- `07-Spring-Boot-Exception-Handling.md`
- `06-Spring-Boot-REST-APIs.md`
- `27-Spring-Boot-Swagger-OpenAPI.md`

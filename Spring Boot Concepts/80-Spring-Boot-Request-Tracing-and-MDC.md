# Request Tracing: Correlation IDs and MDC

Distributed debugging needs a **stable identifier** carried across **logs**, **HTTP clients**, and **async** boundaries—often **`X-Request-Id`** or **W3C `traceparent`**.

---

## Incoming HTTP

- **Filter** or **HandlerInterceptor** reads/generates **`X-Request-Id`**.
- Put value into **SLF4J MDC** (`MDC.put("requestId", ...)`) and **clear** in `finally` to avoid **thread pool leaks** in servlet containers.

---

## Outgoing calls

- Propagate **`traceparent`** / **B3** headers via **`RestClient`** / **`WebClient`** filters.
- Align with **OpenTelemetry** auto-instrumentation when enabled (`63-Spring-Boot-Observability-Micrometer-OpenTelemetry.md`).

---

## Async and `@Async`

MDC is **thread-local**—copy MDC into **async** worker threads explicitly or use **micrometer context propagation** / **TaskDecorator**.

---

## Related

- [Spring Boot Logging](24-Spring-Boot-Logging.md)
- [Spring Boot Observability Micrometer OpenTelemetry](63-Spring-Boot-Observability-Micrometer-OpenTelemetry.md)

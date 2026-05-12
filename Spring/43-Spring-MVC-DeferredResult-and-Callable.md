# `DeferredResult`, `Callable`, and Async MVC

Spring MVC can **release** servlet container threads while work continues asynchronously via **`DeferredResult<T>`** or **`Callable<T>`**—important for **long polling** and **bridging** to reactive pipelines without full **WebFlux**.

---

## `Callable<T>`

Return **`Callable`** from controller method—MVC runs it on **`TaskExecutor`** (async support enabled).

---

## `DeferredResult<T>`

Producer completes later from **another thread**; supports **timeout** and **error** callbacks.

---

## Timeouts

Configure **async request timeout** at container / Spring Boot level—**orphaned** work still needs **cancellation**.

---

## Related

- [DispatcherServlet and Request Handling](10-DispatcherServlet-and-Request-Handling.md)
- [Spring WebFlux Introduction](26-Spring-WebFlux-Introduction.md)

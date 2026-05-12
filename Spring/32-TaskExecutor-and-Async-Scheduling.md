# TaskExecutor and `@Async` Scheduling (Framework View)

Spring unifies **thread pools** behind **`TaskExecutor`** / **`TaskScheduler`** abstractions—used by **`@Async`**, **MVC async**, **WebSocket** heartbeats, and scheduled jobs.

---

## `TaskExecutor`

- **`ThreadPoolTaskExecutor`** is the common configurable pool (core/max queue, rejection policy).
- **`@Async` methods** return **`void`** or **`Future`/`CompletableFuture`** depending on configuration.

---

## `@EnableAsync` + `@Async`

```java
@Configuration
@EnableAsync
public class AsyncConfig implements AsyncConfigurer {
  @Override
  public Executor getAsyncExecutor() {
    var ex = new ThreadPoolTaskExecutor();
    ex.setCorePoolSize(4);
    ex.setMaxPoolSize(16);
    ex.setQueueCapacity(100);
    ex.initialize();
    return ex;
  }
}
```

---

## Pitfalls

- **`@Async` on same bean** (self-invocation) **does not work** without proxy through another bean.

- **Exception handling**: uncaught exceptions in **`void`** async methods may **swallow** errors—use **`AsyncUncaughtExceptionHandler`**.

- **Transaction boundaries**: **`@Transactional` + `@Async`** rarely compose the way newcomers expect—commit **before** async handoff.

---

## Related

- [Spring Boot Async](../Spring%20Boot%20Concepts/26-Spring-Boot-Async.md)
- [Spring Boot Scheduling](../Spring%20Boot%20Concepts/15-Spring-Boot-Scheduling.md)

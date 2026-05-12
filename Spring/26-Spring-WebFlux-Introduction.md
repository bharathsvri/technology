# Spring WebFlux Introduction

**WebFlux** is Spring’s **reactive-stack** web framework built on **Reactor** (`Mono`, `Flux`) and **Netty** (default) instead of servlet API blocking threads.

---

## When WebFlux helps

- High **concurrency** with **I/O-bound** work and **non-blocking** drivers (R2DBC, reactive Mongo, WebClient).
- **Backpressure** scenarios with streaming responses.

## When to stay on MVC

- Heavy **blocking JDBC** / legacy libraries without reactive alternatives.
- Team unfamiliar with reactive debugging (stack traces, thread hops).

---

## Controllers returning `Mono` / `Flux`

```java
@RestController
public class UserController {
    @GetMapping("/users/{id}")
    public Mono<User> get(@PathVariable String id) {
        return repo.findById(id);
    }
}
```

---

## Router functions

Alternative to `@RequestMapping` — see next doc.

---

## Security

**`spring-security-webflux`** provides reactive filter chain (`SecurityWebFilterChain`).

---

## Next

- [WebFlux Router Functions](27-WebFlux-Router-Functions.md)

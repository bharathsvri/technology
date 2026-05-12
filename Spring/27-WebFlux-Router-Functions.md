# WebFlux Router Functions

Functional routing defines endpoints as **`RouterFunction<ServerResponse>`** instead of `@RequestMapping` methods—useful for **modular** route tables and **handler classes** without annotations.

---

## Example

```java
@Configuration
public class UserRoutes {
    @Bean
    public RouterFunction<ServerResponse> routes(UserHandler handler) {
        return RouterFunctions.route()
            .GET("/users/{id}", handler::get)
            .POST("/users", handler::create)
            .build();
    }
}
```

```java
@Component
public class UserHandler {
    public Mono<ServerResponse> get(ServerRequest req) {
        String id = req.pathVariable("id");
        // ...
        return ServerResponse.ok().bodyValue(...);
    }
}
```

---

## Nested routes

`nest(path("/api"), builder -> builder...)` groups common prefixes.

---

## Filters

`RouterFunctions.filter()` adds cross-cutting behavior similar to servlet filters.

---

## Static resources

`RouterFunctions.resources("/static/**", new ClassPathResource("static/"))`.

---

## Next

- [WebClient](28-WebClient.md)

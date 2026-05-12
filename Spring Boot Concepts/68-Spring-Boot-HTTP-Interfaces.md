# Spring Boot: HTTP Interfaces (Declarative HTTP Clients)

Spring Framework 6 (with the right classpath arrangement) supports **declarative HTTP clients**: Java interfaces annotated with **`@HttpExchange`** (and related mapping annotations) backed by **`RestClient`** or **`WebClient`** under the hood.

## Why Use Them?
- **Interface-first** outbound API clients with clear method names and types.
- Easier **testing** (mock the interface) and **OpenFeign-like** ergonomics without extra third-party dependencies.

## Pattern (Conceptual)

```java
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.service.annotation.GetExchange;
import org.springframework.web.service.annotation.HttpExchange;

@HttpExchange("/api/users")
public interface UserApiClient {

    @GetExchange("/{id}")
    User getUser(@PathVariable("id") long id);
}
```

You register a **proxy factory bean** (`HttpServiceProxyFactory` + `RestClientAdapter`)—exact bean wiring depends on your Spring Boot version; follow the current reference guide.

## Error Mapping
Combine with **`RestClient`** response status handlers or a small adapter layer so HTTP 4xx/5xx map to typed exceptions your domain understands.

## Related Topics
- `60-Spring-Boot-RestClient.md`
- `37-Spring-Boot-WebFlux.md` (reactive variant)
- `17-Spring-Boot-Microservices.md` (Resilience4j mentions)

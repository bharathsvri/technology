# Spring Boot: RestClient vs WebClient

## Background
Spring Framework 6 introduced **`RestClient`**, a synchronous HTTP API that modernizes the older **`RestTemplate`** (still present but in maintenance mode for many teams).

## When to Use What

| API | Style | Typical Use |
|-----|--------|-------------|
| `RestClient` | Blocking | MVC services calling other HTTP APIs |
| `WebClient` | Reactive | WebFlux stacks, streaming, non-blocking pipelines |

If you enabled **virtual threads**, blocking `RestClient` becomes more attractive at scale than before—still measure.

## Declaring a `RestClient` Bean

```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.client.RestClient;

@Configuration
class HttpClients {
    @Bean
    RestClient paymentsRestClient(RestClient.Builder builder) {
        return builder
            .baseUrl("https://payments.internal")
            .build();
    }
}
```

## Features to Learn
- **URI templates** variables and encoders.
- **Default headers** and **request interceptors** (via builder customizers).
- **Error handling** with `onStatus` predicates to map HTTP errors to typed exceptions.
- **Exchange** vs **retrieve** patterns (retrieve focuses on successful bodies).

## Resilience
For production calls, add:
- **Timeouts** (connect/read).
- **Retries** with idempotency awareness (GET safe; POST usually not without tokens).
- **Circuit breakers** (Resilience4j) at bean or gateway level.

## Testing
Use **`MockRestServiceServer`** against the same `RestClient` bean to assert outbound calls without hitting the network.

## Related Docs
- `37-Spring-Boot-WebFlux.md` – reactive client/server.
- `26-Spring-Boot-Async.md` – async execution boundaries.

# Spring Retry (`@Retryable`)

**Spring Retry** adds declarative **retry** (and recovery) around methods that call **unreliable** resources—networks, external HTTP APIs, messaging brokers.

## Dependency (Illustrative)
Coordinates differ by Spring Boot generation; prefer **Spring Initializr** or current docs. Typical pattern:

- `spring-retry`
- `spring-aspects` (for `@Retryable` proxying)

Enable retry processing with **`@EnableRetry`** on a configuration class.

## Basic Pattern

```java
import org.springframework.retry.annotation.Backoff;
import org.springframework.retry.annotation.Retryable;
import org.springframework.stereotype.Service;

@Service
public class QuoteClient {

    @Retryable(
        retryFor = { java.io.IOException.class },
        maxAttempts = 4,
        backoff = @Backoff(delay = 200, multiplier = 2)
    )
    public String fetchQuote() {
        // call that may fail transiently
        return remote.get();
    }
}
```

## `@Recover`
Define a **fallback** method invoked when retries are exhausted—signature must match exception parameter conventions from Spring Retry docs.

## Safety Rules
- Only retry **idempotent** operations unless downstream semantics are explicitly safe.
- Combine with **timeouts** and **circuit breakers** (`69-Spring-Boot-Resilience4j.md`) so retries do not amplify outages.

## Testing
Use **direct unit tests** of the client with a flaky stub, or configure a **test** `RetryTemplate` with aggressive policies.

## Related Topics
- `26-Spring-Boot-Async.md`
- `60-Spring-Boot-RestClient.md`

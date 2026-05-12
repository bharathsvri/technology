# Spring Boot: Resilience4j (Circuit Breaker, Retry, Rate Limiter)

**Resilience4j** is a lightweight fault-tolerance library for Java that integrates cleanly with Spring Boot for **circuit breaking**, **retries**, **rate limiting**, **bulkheads**, and **timeouts**.

## Why It Exists
Remote calls fail: networks partition, dependencies saturate, versions drift. Resilience patterns **contain** failures so your service degrades gracefully instead of cascading outages.

## Core Concepts
- **Circuit breaker**: opens after failures, half-open probes, closes on success streaks.
- **Retry**: repeat failed calls with backoff + jitter; only for **idempotent** operations unless you know your downstream is safe.
- **Rate limiter**: protect your own service or respect partner quotas.
- **Bulkhead**: cap concurrent calls to a dependency to avoid thread starvation.

## Spring Boot Integration (Illustrative)
Starters historically named like **`spring-cloud-starter-circuitbreaker-resilience4j`** in Spring Cloud stacks, or **`resilience4j-spring-boot3`** style modules in direct Boot apps—**verify coordinates** for your Boot major version.

Declarative style often uses **`@CircuitBreaker`**, **`@Retry`**, **`@RateLimiter`** on service methods with AOP.

## Configuration Tips
- Prefer **timeouts** everywhere (`RestClient`, JDBC, messaging).
- Add **jitter** to retries to avoid thundering herds.
- Export metrics via **Micrometer** (`63-Spring-Boot-Observability-Micrometer-OpenTelemetry.md`).

## Related Topics
- `47-Spring-Boot-Rate-Limiting.md`
- `17-Spring-Boot-Microservices.md`
- `60-Spring-Boot-RestClient.md`

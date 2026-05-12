# Spring Boot: Testcontainers for Integration Tests

## Problem
Integration tests need **real** databases, messaging brokers, and caches—not mocks—for SQL dialect behavior, transactions, and wire protocols.

## Idea
**Testcontainers** starts lightweight **Docker** containers during JUnit tests, waits until ready, and tears them down afterward.

## Typical Stack
- `spring-boot-testcontainers` (Spring Boot 3.1+) or manual Testcontainers JUnit 5 integration.
- JUnit 5 + `@SpringBootTest` for full application context.

## Minimal Pattern (Conceptual)

```java
import org.springframework.boot.test.context.SpringBootTest;
import org.testcontainers.junit.jupiter.Container;
import org.testcontainers.junit.jupiter.Testcontainers;
import org.testcontainers.containers.PostgreSQLContainer;

@SpringBootTest
@Testcontainers
class OrderRepositoryIT {

    @Container
    static PostgreSQLContainer<?> postgres =
        new PostgreSQLContainer<>("postgres:16-alpine");

    // Spring Boot can wire spring.datasource.* from container JDBC URL via dynamic properties
}
```

Use **`@DynamicPropertySource`** (or Spring Boot’s service connection metadata) to inject JDBC URL, username, password into the Spring context.

## Best Practices
- Mark tests with **`@Tag("integration")`** and run them in a separate CI job (Docker required).
- Prefer **fixed image tags** (`postgres:16-alpine`) over `latest`.
- Reuse containers with **singleton containers** pattern for speed when safe.
- Parallel test execution needs **isolated schemas** or databases per worker.

## Alternatives
- **Docker Compose** in CI for multi-service suites.
- **Embedded** databases (H2) for fast unit-ish tests—keep separate from true integration coverage.

## Related Docs
- `13-Spring-Boot-Testing.md`
- `55-Spring-Boot-Integration-Testing.md`

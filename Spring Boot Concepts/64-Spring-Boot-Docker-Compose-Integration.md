# Spring Boot: Docker Compose Integration

Spring Boot can **automatically start** services defined in a Compose file during development, then **stop** them on application shutdown—reducing “run these five containers first” friction.

> This is distinct from writing your own `docker-compose.yml` for production-style stacks (see `16-Spring-Boot-Docker.md`). Here the focus is **Boot’s development integration** with Compose.

## Typical Use Case
Local development needs PostgreSQL, Redis, and Kafka. You keep a **`compose.yaml`** (or `docker-compose.yml`) next to the app; Boot starts dependent services before the context fully initializes (configurable).

## Dependency (Illustrative)
Add the Compose support starter for your Spring Boot major version (coordinate names shifted across generations—check **Spring Initializr** or current reference docs).

## Key Properties (Patterns)
Names vary slightly by version; search your Boot version for exact keys:
- **`spring.docker.compose.enabled`** — toggle integration.
- **`spring.docker.compose.file`** — explicit compose file path(s).
- **`spring.docker.compose.lifecycle-management`** — start-only vs start-and-stop on shutdown.
- **`spring.docker.compose.readiness.timeout`** — wait for health before continuing.

## Service Connection Metadata
Spring Boot can bind **service host/port** from running containers into `spring.datasource.*`, `spring.data.redis.*`, etc., when using **service connection** conventions—reduces copy-pasting ports from `docker ps`.

## Relationship to Testcontainers
- **Compose integration**: primarily **developer workstation** ergonomics.
- **Testcontainers**: **JUnit** integration tests that start disposable containers in CI.

## Caveats
- Requires **Docker** available on the machine.
- CI pipelines may prefer **Testcontainers** or explicit services instead of implicit laptop-only flows.

## Related Topics
- `16-Spring-Boot-Docker.md`
- `59-Spring-Boot-Testcontainers.md`

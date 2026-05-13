# Spring Boot: R2DBC (Reactive Relational Access)

## Background
**R2DBC** (Reactive Relational Database Connectivity) is a SPI for non-blocking access to relational databases. In Spring Boot it pairs naturally with **WebFlux** when you want relational data without blocking JDBC calls on reactive threads.

**JDBC** (`spring-boot-starter-jdbc`, JPA, `JdbcClient`) is still the default for most **Spring MVC** services. Prefer R2DBC only when your stack is intentionally reactive end-to-end or you are isolating blocking I/O behind schedulers with a clear design.

## When to Choose R2DBC
- You expose **reactive** endpoints (`RouterFunction` / `WebFlux`) and want **relational** storage without wrapping JDBC in `subscribeOn`.
- You need **streaming** large result sets with backpressure-friendly consumption.

## When to Avoid (or Be Careful)
- Typical **blocking MVC** apps: use **JPA / JDBC / Spring Data JDBC** instead (`09-Spring-Boot-Data-JPA.md`, `66-Spring-Boot-JdbcClient.md`, `67-Spring-Boot-Spring-Data-JDBC.md`).
- **Distributed transactions (2PC)** and many legacy ORM patterns map poorly; model explicit units of work.

## Boot Starters (Conceptual)
- **`spring-boot-starter-data-r2dbc`** — `DatabaseClient`, Spring Data R2DBC repositories, dialect integration.
- Pair with a **driver** artifact for your database (vendor-specific R2DBC driver coordinates change over time; use the current Spring Boot dependency management BOM).

## Core APIs to Learn
- **`DatabaseClient`** — fluent SQL execution returning `Mono` / `Flux`.
- **`R2dbcEntityTemplate`** — entity-oriented operations where you do not use repositories.
- **Spring Data R2DBC repositories** — `ReactiveCrudRepository` style for simple CRUD.
- **Transactions** — reactive transactions use **`TransactionalOperator`** or **`@Transactional`** on reactive service methods that return `Mono` / `Flux` (ensure your transaction manager is the reactive R2DBC variant).

## Schema and Migrations
R2DBC does not manage schema. Continue using **Flyway / Liquibase** (`28-Spring-Boot-Database-Migrations.md`). Apply migrations with JDBC tooling or dual configuration; many teams run Flyway with a normal JDBC URL at startup and use R2DBC only for application queries.

## Connection Pooling
Use an **R2DBC pool** implementation compatible with your driver. Tune **max size**, **acquisition timeout**, and validate **health checks** for Kubernetes (`70-Spring-Boot-Availability-Probes.md`, `79-Spring-Boot-HikariCP-and-Connection-Pool-Tuning.md` discusses JDBC pools; apply analogous discipline for reactive pools).

## Testing
- **`@DataR2dbcTest`** slice tests for R2DBC components.
- **`Testcontainers`** with an R2DBC URL (`59-Spring-Boot-Testcontainers.md`) for integration tests.

## Related Docs
- `37-Spring-Boot-WebFlux.md` — reactive web stack and SSE patterns.
- `10-Spring-Boot-Transactions.md` — transactional thinking (imperative vs reactive differs in wiring).
- `63-Spring-Boot-Observability-Micrometer-OpenTelemetry.md` — trace reactive chains.

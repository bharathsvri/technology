# Spring Boot Concepts — Index

This folder collects practical Spring Boot topics aligned with **modern production guidance** (Spring Boot 3.x, Java 17+, observability, containers, and security). Use the list below as a single table of contents.

## Production-oriented themes (from industry checklists)

- Structured **logging** with correlation IDs and JSON where appropriate (`24-Spring-Boot-Logging.md`, Java: `112_SLF4J_MDC_and_Structured_Logging.md`).
- **Metrics and distributed tracing** via Micrometer and OpenTelemetry (`63-Spring-Boot-Observability-Micrometer-OpenTelemetry.md`, `14-Spring-Boot-Actuator.md`).
- **RFC 9457 Problem Details** for consistent API errors (`62-Spring-Boot-Problem-Details-RFC9457.md`).
- **Database migrations**, connection pools, and transactions (`28-Spring-Boot-Database-Migrations.md`, `09-Spring-Boot-Data-JPA.md`, `10-Spring-Boot-Transactions.md`).
- **OAuth2 / Spring Security** for APIs and clients (`57-Spring-Boot-OAuth2-and-Spring-Security.md`, `12-Spring-Boot-Security.md`).
- **Virtual threads** where I/O-bound workloads benefit (`58-Spring-Boot-Virtual-Threads.md`).
- **Testcontainers** and integration tests (`59-Spring-Boot-Testcontainers.md`, `55-Spring-Boot-Integration-Testing.md`).
- **Graceful shutdown** aligned with Kubernetes rollouts (`61-Spring-Boot-Graceful-Shutdown.md`).
- **SBOM** generation for supply-chain visibility (`65-Spring-Boot-SBOM-Supply-Chain.md`).
- **Docker** images and **Compose** ergonomics (`16-Spring-Boot-Docker.md`, `64-Spring-Boot-Docker-Compose-Integration.md`).
- **JdbcClient / Spring Data JDBC** for explicit SQL (`66-Spring-Boot-JdbcClient.md`, `67-Spring-Boot-Spring-Data-JDBC.md`).
- **Declarative HTTP clients** (`68-Spring-Boot-HTTP-Interfaces.md`) and **Resilience4j** fault tolerance (`69-Spring-Boot-Resilience4j.md`).
- **Liveness/readiness** through Actuator availability (`70-Spring-Boot-Availability-Probes.md`).
- **Modular monolith** boundaries with Spring Modulith (`71-Spring-Boot-Spring-Modulith.md`).
- **Declarative retries** with Spring Retry (`72-Spring-Boot-Spring-Retry.md`), **TLS bundles** (`73-Spring-Boot-SSL-Bundles.md`), **cluster-safe cron** with ShedLock (`74-Spring-Boot-ShedLock-Distributed-Scheduling.md`), and **slice MVC tests** (`75-Spring-Boot-MockMvc-and-WebMvcTest.md`).

## All topics (numbered)

- [Introduction to Spring Boot](01-Introduction-to-Spring-Boot.md)
- [Spring Boot Auto Configuration](02-Spring-Boot-Auto-Configuration.md)
- [Spring Boot Starters](03-Spring-Boot-Starters.md)
- [Spring Boot Application Properties](04-Spring-Boot-Application-Properties.md)
- [Spring Boot Profiles](05-Spring-Boot-Profiles.md)
- [Spring Boot REST APIs](06-Spring-Boot-REST-APIs.md)
- [Spring Boot Exception Handling](07-Spring-Boot-Exception-Handling.md)
- [Spring Boot Validation](08-Spring-Boot-Validation.md)
- [Spring Boot Data JPA](09-Spring-Boot-Data-JPA.md)
- [Spring Boot Transactions](10-Spring-Boot-Transactions.md)
- [Spring Boot Caching](11-Spring-Boot-Caching.md)
- [Spring Boot Security](12-Spring-Boot-Security.md)
- [Spring Boot Testing](13-Spring-Boot-Testing.md)
- [Spring Boot Actuator](14-Spring-Boot-Actuator.md)
- [Spring Boot Scheduling](15-Spring-Boot-Scheduling.md)
- [Spring Boot Docker](16-Spring-Boot-Docker.md)
- [Spring Boot Microservices](17-Spring-Boot-Microservices.md)
- [Spring Boot Email](18-Spring-Boot-Email.md)
- [Spring Boot File Upload Download](19-Spring-Boot-File-Upload-Download.md)
- [Spring Boot Thymeleaf](20-Spring-Boot-Thymeleaf.md)
- [Spring Boot WebSocket](21-Spring-Boot-WebSocket.md)
- [Spring Boot AOP](22-Spring-Boot-AOP.md)
- [Spring Boot Internationalization](23-Spring-Boot-Internationalization.md)
- [Spring Boot Logging](24-Spring-Boot-Logging.md)
- [Spring Boot DevTools](25-Spring-Boot-DevTools.md)
- [Spring Boot Async](26-Spring-Boot-Async.md)
- [Spring Boot Swagger OpenAPI](27-Spring-Boot-Swagger-OpenAPI.md)
- [Spring Boot Database Migrations](28-Spring-Boot-Database-Migrations.md)
- [Spring Boot Redis](29-Spring-Boot-Redis.md)
- [Spring Boot MongoDB](30-Spring-Boot-MongoDB.md)
- [Spring Boot RabbitMQ](31-Spring-Boot-RabbitMQ.md)
- [Spring Boot Application Events](32-Spring-Boot-Application-Events.md)
- [Spring Boot Command Line Runner](33-Spring-Boot-Command-Line-Runner.md)
- [Spring Boot Filters Interceptors](34-Spring-Boot-Filters-Interceptors.md)
- [Spring Boot Best Practices](35-Spring-Boot-Best-Practices.md)
- [Spring Boot HATEOAS](36-Spring-Boot-HATEOAS.md)
- [Spring Boot WebFlux](37-Spring-Boot-WebFlux.md)
- [Spring Boot Kafka](38-Spring-Boot-Kafka.md)
- [Spring Boot GraphQL](39-Spring-Boot-GraphQL.md)
- [Spring Boot Performance Optimization](40-Spring-Boot-Performance-Optimization.md)
- [Spring Boot Custom Auto Configuration](41-Spring-Boot-Custom-Auto-Configuration.md)
- [Spring Boot Error Pages](42-Spring-Boot-Error-Pages.md)
- [Spring Boot Resource Handling](43-Spring-Boot-Resource-Handling.md)
- [Spring Boot Conditional Beans](44-Spring-Boot-Conditional-Beans.md)
- [Spring Boot Multi Tenancy](45-Spring-Boot-Multi-Tenancy.md)
- [Spring Boot Batch Processing](46-Spring-Boot-Batch-Processing.md)
- [Spring Boot Rate Limiting](47-Spring-Boot-Rate-Limiting.md)
- [Spring Boot API Versioning](48-Spring-Boot-API-Versioning.md)
- [Spring Boot Content Negotiation](49-Spring-Boot-Content-Negotiation.md)
- [Spring Boot Custom Annotations](50-Spring-Boot-Custom-Annotations.md)
- [Spring Boot Health Indicators](51-Spring-Boot-Health-Indicators.md)
- [Spring Boot Data REST](52-Spring-Boot-Data-REST.md)
- [Spring Boot Message Converters](53-Spring-Boot-Message-Converters.md)
- [Spring Boot View Resolvers](54-Spring-Boot-View-Resolvers.md)
- [Spring Boot Integration Testing](55-Spring-Boot-Integration-Testing.md)
- [Spring Boot Annotations Complete Guide](56-Spring-Boot-Annotations-Complete-Guide.md)
- [Spring Boot OAuth2 and Spring Security](57-Spring-Boot-OAuth2-and-Spring-Security.md)
- [Spring Boot Virtual Threads](58-Spring-Boot-Virtual-Threads.md)
- [Spring Boot Testcontainers](59-Spring-Boot-Testcontainers.md)
- [Spring Boot RestClient](60-Spring-Boot-RestClient.md)
- [Spring Boot Graceful Shutdown](61-Spring-Boot-Graceful-Shutdown.md)
- [Spring Boot Problem Details RFC9457](62-Spring-Boot-Problem-Details-RFC9457.md)
- [Spring Boot Observability Micrometer OpenTelemetry](63-Spring-Boot-Observability-Micrometer-OpenTelemetry.md)
- [Spring Boot Docker Compose Integration](64-Spring-Boot-Docker-Compose-Integration.md)
- [Spring Boot SBOM Supply Chain](65-Spring-Boot-SBOM-Supply-Chain.md)
- [Spring Boot JdbcClient](66-Spring-Boot-JdbcClient.md)
- [Spring Boot Spring Data JDBC](67-Spring-Boot-Spring-Data-JDBC.md)
- [Spring Boot HTTP Interfaces](68-Spring-Boot-HTTP-Interfaces.md)
- [Spring Boot Resilience4j](69-Spring-Boot-Resilience4j.md)
- [Spring Boot Availability Probes](70-Spring-Boot-Availability-Probes.md)
- [Spring Boot Spring Modulith](71-Spring-Boot-Spring-Modulith.md)
- [Spring Boot Spring Retry](72-Spring-Boot-Spring-Retry.md)
- [Spring Boot SSL Bundles](73-Spring-Boot-SSL-Bundles.md)
- [Spring Boot ShedLock Distributed Scheduling](74-Spring-Boot-ShedLock-Distributed-Scheduling.md)
- [Spring Boot MockMvc and WebMvcTest](75-Spring-Boot-MockMvc-and-WebMvcTest.md)

## Related material

- **Spring Framework (non-Boot)**: [Spring](../Spring/README.md)
- Java fundamentals and libraries: `../Java Concepts/00_Index.md`
- SQL Server database topics: `../DBMS/README.md`

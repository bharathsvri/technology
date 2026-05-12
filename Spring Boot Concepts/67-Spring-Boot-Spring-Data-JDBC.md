# Spring Boot: Spring Data JDBC

**Spring Data JDBC** is an opinionated persistence layer that maps aggregates to tables with **explicit persistence**—unlike JPA, there is **no first-level cache**, **no lazy loading proxies**, and **no dirty tracking** beyond aggregate boundaries.

## Mental Model
- Aggregate root = consistency boundary (DDD style).
- References to other aggregates are stored as **IDs**, not navigable graphs.
- Optimistic locking via **`@Version`** columns is supported.

## Starter
Use **`spring-boot-starter-data-jdbc`** (coordinates vary slightly by Boot generation—generate from Spring Initializr).

## When Spring Data JDBC Fits
- Clear **bounded aggregates** and mostly **explicit SQL** via `JdbcAggregateTemplate` / repositories.
- You want **predictable** SQL and fewer ORM surprises than a large JPA model.

## When JPA Fits Better
- Rich **associations**, lazy loading, and vendor-specific query capabilities are central to the domain.

## Migrations
Still use **Flyway/Liquibase** (`28-Spring-Boot-Database-Migrations.md`); Spring Data JDBC does not replace schema management.

## Related Topics
- `66-Spring-Boot-JdbcClient.md` (lower-level JDBC ergonomics)
- `09-Spring-Boot-Data-JPA.md`
- `10-Spring-Boot-Transactions.md`

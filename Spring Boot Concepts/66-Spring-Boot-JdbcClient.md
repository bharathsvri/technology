# Spring Boot: `JdbcClient` (Spring Framework 6.1+)

`JdbcClient` is a small fluent API over **`JdbcTemplate`** / **`NamedParameterJdbcTemplate`** that reduces boilerplate for common JDBC access patterns in Spring Boot apps.

## When to Use
- You chose **JDBC** (or mix JDBC with JPA for hot paths) and want concise reads/writes without a full ORM.
- You want **named parameters** with readable call chains.

## Boot Wiring
With **`spring-boot-starter-jdbc`**, Spring Boot auto-configures a `DataSource`, `JdbcTemplate`, and a **`JdbcClient`** bean you can inject.

## Sketch

```java
import org.springframework.jdbc.core.simple.JdbcClient;

public class OrderRepository {
    private final JdbcClient jdbc;

    public OrderRepository(JdbcClient jdbc) {
        this.jdbc = jdbc;
    }

    public Optional<Order> findById(long id) {
        return jdbc.sql("SELECT id, total FROM orders WHERE id = :id")
                .param("id", id)
                .query(Order.class)
                .optional();
    }
}
```

## Compared to JPA
- **JDBC**: maximal control, minimal magic; you own SQL and mapping.
- **JPA**: great for entity graphs and vendor-independent mapping—often heavier for reporting-style SQL.

## Testing
Use **`@JdbcTest`** slice or Testcontainers-backed `DataSource` with `@SpringBootTest` depending on how much context you need.

## Related Topics
- `28-Spring-Boot-Database-Migrations.md`
- `09-Spring-Boot-Data-JPA.md`
- Java: `29_JDBC.md`

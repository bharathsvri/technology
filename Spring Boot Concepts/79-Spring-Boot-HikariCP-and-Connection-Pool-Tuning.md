# HikariCP and Connection Pool Tuning

**HikariCP** is the default **JDBC connection pool** in Spring Boot. Interviews expect awareness of **pool sizing**, **timeouts**, and **why** “max pool = number of threads” is a starting heuristic, not the whole story.

---

## Key properties (Spring Boot `spring.datasource.hikari.*`)

| Property | Meaning |
|----------|---------|
| **`maximumPoolSize`** | Upper bound on connections |
| **`minimumIdle`** | Idle connections kept warm |
| **`connectionTimeout`** | Wait for a free connection |
| **`maxLifetime`** | Recycle connections before server kills them |
| **`idleTimeout`** | Retire idle connections |

---

## Sizing heuristics

- **Too small**: threads block on **`connectionTimeout`** under load.
- **Too large**: DB **CPU** and **lock contention** suffer; each connection consumes RAM on DB and app.

**Virtual threads** (Java 21+) increase **concurrent tasks**—revisit pool size vs DB limits; **not** “unbounded pool.”

---

## Observability

Expose **pool metrics** via **Actuator** / **Micrometer** (`hikaricp.connections.active`, etc.) to correlate with latency.

---

## Related

- [Spring Boot Data JPA](09-Spring-Boot-Data-JPA.md)
- [Spring JDBC and JdbcTemplate](../Spring/24-Spring-JDBC-JdbcTemplate.md)

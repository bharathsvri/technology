# `ResourceDatabasePopulator` and `EmbeddedDatabaseBuilder`

Spring’s **JDBC** namespace / Java config supports **`EmbeddedDatabaseBuilder`** (H2, HSQLDB) and **`ResourceDatabasePopulator`** to run **`schema.sql` / `data.sql`** in tests or local dev—distinct from **Flyway** for **ephemeral** databases.

---

## Typical use

- **Integration tests** with **`@JdbcTest`** slice.
- **Prototype** apps before real DB is wired.

---

## Flyway vs embedded scripts

- **Flyway**: versioned migrations for **shared** environments.
- **Populator**: **throwaway** schema bootstrap.

---

## Pitfalls

- **Dialect** differences between embedded engine and **production** SQL Server/Postgres—use **Testcontainers** when fidelity matters.

---

## Related

- [Spring JDBC and JdbcTemplate](24-Spring-JDBC-JdbcTemplate.md)
- [Spring Boot Testcontainers](../Spring%20Boot%20Concepts/59-Spring-Boot-Testcontainers.md)

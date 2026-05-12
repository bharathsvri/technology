# `AbstractRoutingDataSource` and Multi-Tenant Data Sources

**`AbstractRoutingDataSource`** delegates **`getConnection()`** to a **target** `DataSource` chosen at runtime—common for **tenant-per-schema**, **read replicas**, or **shard** routing.

---

## Pattern

1. Hold **map** of keys → real **`DataSource`** beans.
2. Determine key from **`ThreadLocal`**, **security context**, or **request header**.
3. Implement **`determineCurrentLookupKey()`** returning that key.

---

## Transaction boundaries

Routing must be **stable** for the duration of a **transaction**—changing lookup mid-tx breaks atomicity expectations.

---

## Spring Boot wiring

Define **routing** `DataSource` as **`@Primary`** bean that wraps **delegates**; configure **JPA**/`JdbcTemplate` to use it.

---

## Related

- [Spring JDBC and JdbcTemplate](24-Spring-JDBC-JdbcTemplate.md)
- [Spring Boot Multi Tenancy](../Spring%20Boot%20Concepts/45-Spring-Boot-Multi-Tenancy.md)

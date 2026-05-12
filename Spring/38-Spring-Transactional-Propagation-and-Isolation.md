# `@Transactional` Propagation and Isolation (Framework View)

Spring’s **`@Transactional`** maps declarative transactions onto **proxy**-wrapped beans. **`propagation`** and **`isolation`** mirror JTA semantics with Spring-specific defaults.

---

## Propagation highlights

| Value | Behavior sketch |
|-------|------------------|
| **`REQUIRED`** (default) | Join existing tx or create new |
| **`REQUIRES_NEW`** | Suspend current, new tx |
| **`NESTED`** | Savepoint nested tx (DataSource-dependent) |
| **`SUPPORTS`** | Join if exists, else non-transactional |
| **`NOT_SUPPORTED`** | Suspend current |

---

## Isolation

Delegates to JDBC **`Connection.setTransactionIsolation`**—see DBMS notes for **phenomena** trade-offs.

---

## Pitfalls

- **Self-invocation** bypasses proxy → annotation ignored.
- **`REQUIRES_NEW`** + long work → **connection pool** pressure if many suspended txs.

---

## Related

- [Spring Transactions](19-Spring-Transactions.md)
- [Spring Boot Transactions](../Spring%20Boot%20Concepts/10-Spring-Boot-Transactions.md)

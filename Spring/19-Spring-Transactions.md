# Programmatic and Declarative Transactions

Spring’s **`PlatformTransactionManager`** abstracts JDBC, JPA, JTA. Most developers use **`@Transactional`** for declarative transactions via AOP **proxies**.

---

## `@Transactional` basics

```java
@Service
public class OrderService {

    @Transactional
    public void placeOrder(OrderId id) {
        // all DB work in same transaction by default
        lines.save(id);
        inventory.reserve(id);
    }
}
```

**Default propagation**: `REQUIRED` — join existing transaction or create new.

**Default isolation**: usually `DEFAULT` (database default).

**`readOnly = true`** — optimization hint for Hibernate/JPA read paths.

---

## Propagation modes (when to use)

| Propagation | Behavior |
|-------------|----------|
| **REQUIRED** (default) | Join or create |
| **REQUIRES_NEW** | Suspend current, always new tx (audit logging, independent failure) |
| **MANDATORY** | Must already exist; else error |
| **NEVER** | Must not exist; else error |
| **NOT_SUPPORTED** | Suspend and run non-transactionally |
| **NESTED** | Savepoint nested tx (JDBC savepoints) — rare |

---

## Rollback rules

Default: rollback on **unchecked** exceptions only.

```java
@Transactional(rollbackFor = {IOException.class})
```

---

## Self-invocation problem

Calling `this.placeOrder()` from another method **in the same class** **bypasses** the proxy — **`@Transactional` will not apply**. Fix: extract to another bean or inject self proxy (`@Lazy` self) carefully.

---

## `TransactionTemplate`

Programmatic control:

```java
txTemplate.executeWithoutResult(status -> {
    // work
});
```

Useful in non-public methods or non-Spring-managed classes (still need access to template).

---

## Transaction manager selection

Boot auto-picks **`DataSourceTransactionManager`** (JDBC) or **`JpaTransactionManager`** when JPA on classpath. Multiple `DataSource` → configure **chained** or **JTA** for XA.

---

## Next

- [Application Events](20-Application-Events.md)

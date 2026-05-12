# Inversion of Control (IoC)

## Definition

**Inversion of Control** means the **framework** (the Spring container) controls the **flow** and **lifecycle** of key application objects, instead of your code manually constructing the whole object graph with `new`.

You **invert** who is in control:

| Traditional control | Inverted (Spring) control |
|---------------------|---------------------------|
| Your `main()` creates `Service`, `Repository`, `DataSource` in the right order | Spring reads **metadata** (annotations, `@Bean` methods, XML) and creates beans in a **safe order** |
| You pass dependencies by hand | Spring **injects** them based on types and qualifiers |
| You call lifecycle hooks ad hoc | Spring calls **`@PostConstruct`**, **`DisposableBean`**, etc. consistently |

---

## Example: without IoC

```java
public class OrderService {
    private final OrderRepository repo;

    public OrderService() {
        var ds = createDataSource();           // configuration noise
        this.repo = new JdbcOrderRepository(ds); // concrete coupling
    }
}
```

Problems:

- Hard to **unit test** (always hits real JDBC).
- Hard to **swap** implementations (reporting vs OLTP).
- Configuration is **scattered** and duplicated.

---

## Example: with IoC

```java
@Service
public class OrderService {
    private final OrderRepository repo;

    public OrderService(OrderRepository repo) { // Spring supplies the bean
        this.repo = repo;
    }
}
```

Spring creates (or reuses) a `OrderRepository` bean—often an interface with a JDBC or JPA implementation registered elsewhere.

---

## Benefits (why teams adopt IoC)

1. **Testability** — pass a mock `OrderRepository` in tests without subclassing.
2. **Separation of concerns** — business code does not embed environment-specific wiring.
3. **Centralized configuration** — one place to change URLs, pool sizes, feature flags.
4. **Consistent lifecycle** — singleton vs prototype, init/destroy, shutdown ordering.

---

## IoC vs Dependency Injection (DI)

- **IoC** is the *big idea*: control is inverted to the framework.
- **DI** is the *main technique* Spring uses: dependencies are **injected** into your classes.

People often use the terms together; the next document focuses on **DI patterns** in Spring.

---

## Common misconceptions

- **“IoC means no `new` ever”** — you still `new` value objects, DTOs, and local helpers; you avoid `new` for **long-lived, injectable services**.
- **“Spring is magic”** — it is mostly **reflection + a well-defined lifecycle**; learning that lifecycle removes the mystery.

---

## Next

- [Dependency Injection](03-Dependency-Injection.md) — constructor vs field injection, `@Qualifier`, `@Primary`.

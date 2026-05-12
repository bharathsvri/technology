# Dependency Injection (DI)

**Dependency Injection** means an object’s dependencies are **provided from outside** (by the Spring container) instead of the object creating them internally. Spring supports **constructor**, **setter**, and **field** injection; **constructor injection is the recommended default** for required dependencies.

---

## 1. Constructor injection (recommended)

```java
@Service
public class OrderService {

    private final OrderRepository orders;
    private final PricingService pricing;

    public OrderService(OrderRepository orders, PricingService pricing) {
        this.orders = orders;
        this.pricing = pricing;
    }

    public BigDecimal totalForCustomer(String customerId) {
        var lines = orders.findLines(customerId);
        return pricing.price(lines);
    }
}
```

### Why prefer the constructor?

| Benefit | Explanation |
|---------|-------------|
| **Immutability** | Fields can be `final` — dependencies cannot be reassigned accidentally. |
| **Testability** | Unit tests call `new OrderService(mockRepo, mockPricing)` with no Spring context. |
| **Fail fast** | If a dependency is missing, the object **cannot be constructed** at all. |
| **Clear contract** | The constructor signature documents **everything** the class needs. |

### `@Autowired` on constructor

From **Spring Framework 4.3+**, if a class has **only one** constructor, Spring uses it for injection **without** `@Autowired`. You may still annotate for clarity in large codebases.

```java
@Autowired // optional when this is the only constructor
public OrderService(OrderRepository orders) { ... }
```

---

## 2. Setter injection

Use when a dependency is **optional** or must be **reconfigured** after construction (rare in modern apps).

```java
@Service
public class ReportService {
    private MessageFormatter formatter = new DefaultFormatter();

    @Autowired(required = false)
    public void setFormatter(MessageFormatter formatter) {
        if (formatter != null) {
            this.formatter = formatter;
        }
    }
}
```

**Caution**: partial construction — the object may exist in an inconsistent state until setters run. Prefer constructor + optional **wrapper** or **Null Object** pattern when possible.

---

## 3. Field injection (`@Autowired` on field)

```java
@Service
public class LegacyService {
    @Autowired
    private OrderRepository orders; // not final
}
```

### Downsides

- Harder to **unit test** without `ReflectionTestUtils` or Spring’s test context.
- Hides dependencies — readers do not see them in the constructor.
- Cannot use `final` fields.

**Guideline**: avoid for **required** dependencies; legacy code may still use it.

---

## Resolving multiple beans of the same type

### `@Primary`

Marks the **default** bean when autowiring by type finds multiple candidates.

```java
@Bean @Primary
public NotificationSender emailSender() { ... }

@Bean
public NotificationSender smsSender() { ... }

// Injects emailSender unless qualified otherwise
@Autowired NotificationSender sender;
```

### `@Qualifier`

Disambiguate by **bean name** or custom qualifier annotation.

```java
public OrderService(@Qualifier("smsSender") NotificationSender urgentSender) { ... }
```

### Custom `@Qualifier` annotations

For clearer APIs than string names:

```java
@Target({ElementType.FIELD, ElementType.PARAMETER})
@Retention(RetentionPolicy.RUNTIME)
@Qualifier
public @interface UrgentChannel { }
```

---

## Interface-based design

Program to **interfaces**; register **implementations** as beans:

```java
public interface OrderRepository {
    List<OrderLine> findLines(String customerId);
}
```

```java
@Repository
public class JdbcOrderRepository implements OrderRepository { ... }
```

`OrderService` depends on `OrderRepository`, not `JdbcOrderRepository` — easier to mock and swap.

---

## Circular dependencies

If `A` needs `B` and `B` needs `A`, constructor injection can fail at startup. Fixes:

- Refactor to extract a third bean `C` holding shared behavior.
- Use **`@Lazy`** on one side (injects a proxy resolved later) — use sparingly; can hide design smells.
- Prefer **setter** injection only as a last resort for legacy cycles.

---

## Summary

| Style | When |
|-------|------|
| **Constructor** | Default for all **required** dependencies |
| **Setter** | Optional / reconfigurable dependencies |
| **Field** | Avoid for new code |
| **`@Primary` / `@Qualifier`** | Multiple beans of the same type |

---

## Next

- [ApplicationContext and BeanFactory](04-ApplicationContext-and-BeanFactory.md) — the object that performs injection at runtime.

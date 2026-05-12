# Application Events

Spring’s **`ApplicationEventPublisher`** lets beans publish **domain events** decoupled from listeners. Useful for **audit**, **notifications**, **projections**, and **cache eviction** after commits.

---

## Publishing events

```java
@Service
public class OrderService {
    private final ApplicationEventPublisher publisher;

    public OrderService(ApplicationEventPublisher publisher) {
        this.publisher = publisher;
    }

    public void ship(OrderId id) {
        // ... business logic ...
        publisher.publishEvent(new OrderShippedEvent(id));
    }
}
```

Any **`ApplicationListener`** or **`@EventListener`** method receives compatible event types.

---

## `@EventListener`

```java
@Component
public class OrderListeners {
    @EventListener
    public void onShipped(OrderShippedEvent e) {
        // handle
    }
}
```

Supports **SpEL conditions**, **async** execution (`@Async`), and ordering (`@Order`).

---

## `@TransactionalEventListener`**

Runs listener **after commit** (or before commit / after rollback) — avoids sending emails if transaction rolls back.

```java
@TransactionalEventListener(phase = TransactionPhase.AFTER_COMMIT)
public void notifyWarehouse(OrderShippedEvent e) { ... }
```

---

## Async events

Enable **`@EnableAsync`** and mark listener `@Async` — errors need separate handling (uncaught async exceptions).

---

## Event type hierarchy

Listeners can accept **superclass** events if using generic resolution rules—design clear event types to avoid accidental broad catches.

---

## Next

- [Resource Abstraction](21-Resource-Abstraction.md)

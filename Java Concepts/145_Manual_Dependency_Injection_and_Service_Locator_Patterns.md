# Manual dependency injection and service locator patterns (without Spring)

## Why this topic exists
Frameworks like Spring hide composition behind annotations and autowiring (`70_Spring_Framework_Basics.md`). Plain Java libraries still need **wiring**: constructing graphs of implementations, swapping fakes in tests, and avoiding **global singletons**.

## Constructor injection (preferred)
Pass dependencies as **constructor parameters** or **static factory parameters**. The object is unusable until required collaborators exist—failures become **compile-time** or immediate construction errors.

```java
public final class OrderService {
    private final PaymentClient payments;
    private final OrderRepository orders;

    public OrderService(PaymentClient payments, OrderRepository orders) {
        this.payments = Objects.requireNonNull(payments);
        this.orders = Objects.requireNonNull(orders);
    }
}
```

## Factory or builder modules
A small **`Application`** or **`CompositionRoot`** class (often beside `main`) is responsible for **choosing implementations** based on configuration: in-memory repositories in dev, JDBC in prod.

## Service locator (use sparingly)
A registry that resolves dependencies by **type or name** at runtime. It trades explicit graphs for **hidden coupling** and complicates testing unless boundaries are narrow.

Reserve locators for **plugin systems** (`122_ServiceLoader_and_SPI_Patterns.md`) or legacy constraints, not as the default DI style.

## Testing payoff
Constructor injection lets tests pass **`MockPaymentClient`** without bytecode magic (`103_Mockito_Advanced.md` still helps for partial mocks).

## Related docs
- `30_Design_Patterns.md` — broader creational patterns
- `113_OOP_SOLID_Principles.md` — DIP and interface segregation
- `70_Spring_Framework_Basics.md` — when a container adds value

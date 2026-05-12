# Bean Lifecycle and Scopes

Spring beans have a **lifecycle**: instantiation → dependency injection → initialization callbacks → use → destruction. **Scope** controls how many instances exist and how long they live.

---

## Bean scopes (core)

| Scope | Meaning | Typical use |
|-------|---------|-------------|
| **singleton** (default) | One shared instance per container | Services, repositories, config |
| **prototype** | **New** instance every injection / `getBean` | Stateful non-singleton objects; Spring does **not** call full destroy lifecycle on prototypes the same way |
| **request** | One instance per **HTTP request** (web) | Request-scoped “user context” |
| **session** | One per HTTP **session** | Shopping cart backing bean |
| **application** | One per `ServletContext` | Shared read-only caches tied to app lifetime |

**Request/session/application** scopes require a **scoped proxy** when injected into singletons (Spring creates a CGLIB/JDK proxy that resolves the real instance per request/session).

```java
@Bean
@SessionScope
public UserPreferences userPreferences() {
    return new UserPreferences();
}
```

---

## Initialization callbacks

Order (simplified):

1. Bean instantiated (constructor).
2. Dependencies injected.
3. **`BeanNameAware.setBeanName`** (if implemented).
4. **`BeanFactoryAware` / `ApplicationContextAware`** (if implemented).
5. **`@PostConstruct`** (JSR-250) — common choice.
6. **`InitializingBean.afterPropertiesSet()`** — older style.
7. Custom **`init-method`** on `@Bean`.

Prefer **`@PostConstruct`** or **`@Bean(initMethod=...)`** for clarity.

```java
@PostConstruct
public void warmCache() {
    // safe: dependencies are already injected
}
```

---

## Destruction callbacks

1. **`@PreDestroy`**
2. **`DisposableBean.destroy()`**
3. **`@Bean(destroyMethod="close")`** — common for `DataSource`, `SessionFactory`.

**Singleton** beans receive destruction when the context closes. **Prototype** beans: you must clean up yourself or use a custom holder.

---

## `BeanPostProcessor`

Advanced extension point: intercept **every** bean before/after initialization. Used internally for **`@Autowired`**, **AOP proxies**, **validation**, etc. Custom `BeanPostProcessor` beans run early and can wrap beans in proxies.

---

## Lazy initialization

`@Lazy` on a bean or injection point delays creation until first use — can speed **startup** but moves failures to **first request**.

---

## Pitfalls

- **Prototype injected into singleton** — singleton holds a **reference** created once unless you use a **`ObjectFactory<T>`** / **`Provider<T>`** / scoped proxy to obtain a fresh prototype per use.
- **`final` fields** — cannot reassign after construction; use constructor injection for immutable dependencies.

---

## Next

- [Configuration and Component Scanning](06-Configuration-and-Component-Scanning.md)

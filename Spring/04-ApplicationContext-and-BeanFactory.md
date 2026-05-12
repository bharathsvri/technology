# ApplicationContext and BeanFactory

Spring’s IoC container is exposed mainly through **`BeanFactory`** (minimal API) and **`ApplicationContext`** (enterprise superset). In almost all applications you work with an **`ApplicationContext`** implementation.

---

## BeanFactory (low-level)

- **`getBean(String name)`**, **`getBean(Class)`** — retrieve beans.
- **`containsBean`**, **`isSingleton`**, **`getAliases`** — introspection.
- By default, **singleton** beans are created **lazily** on first `getBean` unless configured otherwise.

Use `BeanFactory` directly only in embedded or framework-style code; applications inject dependencies instead.

---

## ApplicationContext (what you actually use)

`ApplicationContext` **extends** `BeanFactory` and adds:

| Capability | Typical use |
|------------|-------------|
| **Eager singleton init** | Fail fast at startup if wiring is broken |
| **`MessageSource`** | i18n messages |
| **`ResourcePatternResolver`** | `classpath*:/**/*.xml` loading |
| **`ApplicationEventPublisher`** | `publishEvent` for domain events |
| **`Environment`** | profiles, `PropertySource` chain |
| **Web integration** | `WebApplicationContext` ties to `ServletContext` |

---

## Common implementations

| Class | Typical scenario |
|-------|------------------|
| **`AnnotationConfigApplicationContext`** | Standalone Java app, tests, CLI — pass `@Configuration` classes |
| **`ClassPathXmlApplicationContext`** | Legacy XML configuration |
| **`FileSystemXmlApplicationContext`** | XML paths on file system |
| **`AnnotationConfigWebApplicationContext`** | Servlet 3+ Java config web app |
| **`SpringApplication` (Boot)** | Boot wraps creation of the right `ApplicationContext` |

### Example: standalone Java config

```java
try (var ctx = new AnnotationConfigApplicationContext(AppConfig.class)) {
    OrderService orders = ctx.getBean(OrderService.class);
    orders.place(...);
}
```

In Boot you rarely write this — `SpringApplication.run` does it — but the **same** `ApplicationContext` types exist underneath.

---

## Bean definition names

Every bean has at least one **name** (default is derived from class name with lower-camelCase). `@Bean` methods use **method name** unless overridden:

```java
@Bean("primaryDataSource")
public DataSource dataSource() { ... }
```

---

## `getBean` vs injection

**Prefer injection** (`@Autowired` constructor, `@Inject`, or plain constructor in Spring-managed classes). Use **`getBean`** only when:

- Writing **framework** code that must resolve beans dynamically.
- Integrating **non-Spring** code that needs one-off access to the context.

Overuse of `getBean` scatters **service locator** anti-pattern across the codebase.

---

## Context hierarchy (web)

A **parent/child** context chain is possible: e.g. **root** context for services, **child** context for web MVC layer. Most Boot apps use a **single** flattened context unless you configure a hierarchy explicitly.

---

## Shutdown hook

`ConfigurableApplicationContext.registerShutdownHook()` (Boot does this) ensures **DisposableBean** / `@PreDestroy` runs on JVM exit.

---

## Next

- [Bean Lifecycle and Scopes](05-Bean-Lifecycle-and-Scopes.md)

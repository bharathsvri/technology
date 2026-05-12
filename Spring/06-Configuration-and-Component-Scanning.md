# Configuration and Component Scanning

Spring discovers beans from **Java `@Configuration` classes**, **XML**, or **Groovy DSL**. Modern Spring favors **annotation-based** configuration with optional **component scanning**.

---

## `@Configuration` and `@Bean`

A **`@Configuration` class** holds `@Bean` methods that define beans **explicitly**:

```java
@Configuration
public class DataConfig {

    @Bean
    public DataSource dataSource() {
        HikariConfig cfg = new HikariConfig();
        cfg.setJdbcUrl("jdbc:postgresql://localhost/app");
        return new HikariDataSource(cfg);
    }

    @Bean
    public JdbcTemplate jdbcTemplate(DataSource dataSource) {
        return new JdbcTemplate(dataSource);
    }
}
```

### Full `@Configuration` vs `lite` mode

**Full `@Configuration`** (default) uses CGLIB subclassing so that **calling another `@Bean` method from inside the same class** still returns the **singleton** bean from the container—not a new instance.

**`@Configuration(proxyBeanMethods = false)`** (“lite”) skips subclassing: faster startup, but **do not** call `@Bean` methods on `this` from the same class if you need shared singleton semantics—extract to separate `@Bean` methods in another class or inject dependencies.

---

## `@ComponentScan`

Automatically registers classes annotated with stereotype annotations in given **base packages**:

- `@Component`
- `@Service`
- `@Repository` (exception translation for persistence)
- `@Controller` / `@RestController`
- `@Configuration` (also picked up as configuration)

```java
@Configuration
@ComponentScan(basePackages = "com.example.app")
public class ApplicationRootConfig { }
```

### Default package scanning (Boot)

`@SpringBootApplication` is `@Configuration` + `@ComponentScan` on its package **and sub-packages** — placing `Application` in the root package is conventional.

### Avoid over-broad scans

Scanning `com` or the entire classpath slows startup and can register **unintended** beans. Prefer **narrow base packages**.

---

## `@Import`

Compose configurations:

```java
@Configuration
@Import({ MetricsConfig.class, SecurityConfig.class })
public class AppConfig { }
```

Also used for **`ImportSelector`** / **`DeferredImportSelector`** (Boot auto-config uses these heavily).

---

## Conditional registration

- **`@Profile("dev")`** — activate when `spring.profiles.active` includes `dev`.
- **`@ConditionalOn*`** (Spring Boot) — classpath, property, cloud conditions.

---

## Summary

| Mechanism | Use when |
|-----------|----------|
| **`@Bean` methods** | Third-party classes, explicit wiring, multiple beans of same type |
| **`@ComponentScan`** | Your own `@Service` / `@Repository` classes |
| **`@Import`** | Modular config, auto-config, splitting large configs |

---

## Next

- [SpEL](07-SpEL-Spring-Expression-Language.md)

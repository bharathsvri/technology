# Environment, PropertySources, and Profiles

The **`Environment`** abstraction aggregates **`PropertySource`** instances (system properties, env vars, `application.properties`, JNDI, command line) and exposes **active/default profiles**.

---

## Accessing properties

```java
@Value("${app.timeout-ms:5000}")
private long timeoutMs;

// or
@Autowired Environment env;
env.getProperty("app.timeout-ms", Long.class, 5000L);
```

---

## Property precedence (Spring Boot extends heavily)

Boot documents a long ordered list: command line overrides `application.properties`, profiles, etc. For plain Spring, order depends on how you add `PropertySource` beans.

---

## `@PropertySource`

```java
@Configuration
@PropertySource("classpath:extra.properties")
public class ExtraConfig { }
```

Loads classic `.properties` files into the environment (YAML requires `YamlPropertySourceFactory` custom factory).

---

## Profiles

```java
@Bean
@Profile("dev")
public DataSource devDataSource() { ... }
```

Activate with **`spring.profiles.active=dev,postgres`** or `ConfigurableEnvironment.setActiveProfiles`.

---

## `spring.config.import` (Boot)

Cloud config / optional imports—see Boot reference.

---

## Next

- [Spring Testing Framework](23-Spring-Testing-Framework.md)

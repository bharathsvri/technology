# `@ConfigurationProperties` Relaxed Binding

Spring Boot maps **`application.properties`** keys to **Java bean** fields using **relaxed binding** rules: **`my.app-name`**, **`MY_APP_NAME`**, and **`myAppName`** can bind the same property.

---

## Immutable binding (Boot 2.2+)

**`@ConstructorBinding`** (older) / **`record`** or **`@ConfigurationProperties` on `@Bean`** patterns—prefer **immutable** config objects when possible.

---

## Nested properties

Use **inner classes** or **records** for **`my.app.connection-timeout`** style trees.

---

## Pitfalls

- **Unknown properties** silently ignored unless **`ignoreUnknownFields=false`** on legacy setups—validate with **`spring-boot-configuration-processor`** metadata.

---

## Related

- [Spring Boot Application Properties](04-Spring-Boot-Application-Properties.md)
- [Spring Boot Configuration Metadata and IDE Hints](84-Spring-Boot-Configuration-Metadata-and-IDE-Hints.md)

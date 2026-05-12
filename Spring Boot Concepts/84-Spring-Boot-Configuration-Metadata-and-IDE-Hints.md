# `spring-boot-configuration-processor` and IDE Metadata

The **`spring-boot-configuration-processor`** annotation processor generates **`spring-configuration-metadata.json`** so IDEs can **autocomplete** and **document** your **`@ConfigurationProperties`** classes.

---

## Setup (conceptual)

- Add **`annotationProcessor`** dependency (Gradle) or **`optional`** scope (Maven).
- Annotate **`@ConfigurationProperties`** with **`@ConfigurationPropertiesScan`** or explicit **`@EnableConfigurationProperties`**.

---

## `additional-spring-configuration-metadata.json`

Hand-maintain **hints**, **deprecations**, and **value providers** for keys not derived from Java beans.

---

## Interview value

Shows you care about **DX** for **platform** teams shipping internal starters.

---

## Related

- [Spring Boot Application Properties](04-Spring-Boot-Application-Properties.md)
- [Spring Boot Custom Auto Configuration](41-Spring-Boot-Custom-Auto-Configuration.md)

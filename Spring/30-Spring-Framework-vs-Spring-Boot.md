# Spring Framework vs Spring Boot

| Concern | Spring Framework | Spring Boot |
|---------|------------------|-------------|
| **Configuration** | You wire beans explicitly (Java/XML) | **Auto-configuration** + overrides |
| **Embedded server** | Usually deploy WAR to Tomcat | **Embedded** Tomcat/Jetty/Undertow |
| **Entry point** | `web.xml` / `WebApplicationInitializer` | `public static void main` |
| **Dependencies** | You align versions manually (BOM helps) | **Starters** pin tested sets |
| **Production features** | Add Actuator, metrics yourself | **`spring-boot-starter-actuator`**, metrics auto-wiring |

---

## Relationship

Boot is **not a separate framework**—it is **Spring + conventions**:

- Still an **`ApplicationContext`**
- Still **`@Transactional`**, **`@Controller`**, **`JdbcTemplate`**
- Adds **`@SpringBootApplication`**, **`application.yml`**, **`SpringApplication.run`**

---

## When plain Spring still appears

- Shared **libraries** that should not pull Boot starters.
- **Legacy** WARs maintained without Boot migration yet.
- **Fine-grained** control where auto-config would fight you (rare).

---

## Learning path

1. Understand **IoC/DI** and **`@Configuration`** (this `Spring/` folder).
2. Apply **Boot** for real services (`Spring Boot Concepts/` in this repo).
3. Deep dive **Security**, **Data**, **Cloud** as needed.

---

## Official docs

- [Spring Framework reference](https://docs.spring.io/spring-framework/reference/)
- [Spring Boot reference](https://docs.spring.io/spring-boot/reference/)

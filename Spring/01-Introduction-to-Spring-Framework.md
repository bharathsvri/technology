# Introduction to the Spring Framework

## What is Spring?

**Spring** is an open-source **application framework** for Java that helps you build enterprise applications with less boilerplate. At its heart is the **Inversion of Control (IoC) container**, which manages object **lifecycle** and **dependencies** so your classes stay focused on business logic instead of `new` wiring.

Spring is **modular**: you pull in only the jars you need:

| Module (conceptual) | Typical use |
|---------------------|-------------|
| **spring-core** | `BeanFactory`, utilities |
| **spring-context** | `ApplicationContext`, events, SpEL |
| **spring-web** | Servlet integration, web abstractions |
| **spring-webmvc** | `@Controller`, `DispatcherServlet` |
| **spring-webflux** | Reactive stack, `WebClient` |
| **spring-jdbc** | `JdbcTemplate`, exception translation |
| **spring-orm** / **spring-tx** | JPA integration, declarative transactions |
| **spring-security** | Authentication & authorization (separate project, integrates tightly) |

**Spring Boot** is **not** a replacement for Spring—it is an **opinionated layer** on top that auto-configures defaults, embeds Tomcat/Jetty, and ships “starters” so you add one dependency instead of ten.

---

## Spring vs “just Java EE / Jakarta”

Spring predates and often **competes** with parts of Jakarta EE, but they can coexist (e.g. JPA, Bean Validation, Servlet API). Many teams use **Spring Boot + JPA + Hibernate** as their default stack.

---

## Why learn Spring Framework (not only Boot)?

1. **Debugging**: When `@Transactional` does not roll back, or a `@Cacheable` method is not cached, understanding **proxies** and the **core container** saves hours.
2. **Legacy systems**: WAR deployments, XML configs, or hybrid apps still use “plain” Spring wiring.
3. **Libraries**: Open-source tools often expose `ApplicationContext` or `BeanPostProcessor` extension points.
4. **Interviews**: Interviewers still ask how **IoC**, **scopes**, and **MVC dispatch** work under the hood.

---

## Maven coordinates (minimal examples)

Core container (most apps need this or get it transitively):

```xml
<dependency>
  <groupId>org.springframework</groupId>
  <artifactId>spring-context</artifactId>
</dependency>
```

Servlet-based **Spring MVC**:

```xml
<dependency>
  <groupId>org.springframework</groupId>
  <artifactId>spring-webmvc</artifactId>
</dependency>
```

Reactive **WebFlux**:

```xml
<dependency>
  <groupId>org.springframework</groupId>
  <artifactId>spring-webflux</artifactId>
</dependency>
```

In practice, **Spring Boot starters** pull these in with tested versions—see `Spring Boot Concepts` in this repo for Boot-centric docs.

---

## Mental model: beans and the container

- You declare **beans** (objects the container creates and wires).
- The container resolves **constructor/setter dependencies** before giving you a fully usable object graph.
- Cross-cutting concerns (**transactions**, **security**, **caching**) are often applied via **AOP proxies** around your beans.

---

## Official documentation

- [Spring Framework reference](https://docs.spring.io/spring-framework/reference/)
- [Spring Boot reference](https://docs.spring.io/spring-boot/documentation.html) (for deployment and auto-config)

---

## Next in this series

- [Inversion of Control](02-Inversion-of-Control.md) — *why* the container owns object creation.
- [Dependency Injection](03-Dependency-Injection.md) — *how* dependencies are supplied.

# Spring Modulith (Modular Monolith)

**Spring Modulith** structures a single deployable Spring Boot application as **well-bounded modules** (packages or Gradle/Maven modules) with **explicit APIs**, **integration tests per module**, and **documentation** generated from the structure—without splitting into microservices.

## Why Use It?
- **Encapsulation** at scale: prevent “everything calls everything” package tangles.
- **Incremental extraction**: a module that is already isolated is easier to lift into its own service later.
- **Documentation**: generate module canvas / PlantUML from code and events.

## Typical Layout (Illustrative)

```text
com.example.order
  ├── internal/        // not part of public module API
  ├── web/
  ├── persistence/
  └── OrderManagement.java  // @ApplicationModule annotated type or documented facade
```

Rules of thumb:
- Other modules depend only on **public** types in a module’s root package (or documented facades).
- Use **application events** or **internal APIs** instead of reaching into `internal` packages.

## Tooling
- **Spring Modulith** test support verifies **package dependencies** (no illegal cross-module references).
- Event **documentation** and **observability** hooks for domain events.

## When Not Needed
- Very small apps where package discipline is easy without enforcement.

## Related Topics
- `43_Module_System.md` (Java Platform Module System is orthogonal but complementary)
- `17-Spring-Boot-Microservices.md`
- `35-Spring-Boot-Best-Practices.md`

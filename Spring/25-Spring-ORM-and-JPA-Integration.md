# Spring ORM and JPA Integration

Spring integrates **JPA** (`EntityManager`, `EntityManagerFactory`) so repositories can stay **`@Transactional`** and exception-translated.

---

## `LocalContainerEntityManagerFactoryBean`

Bootstraps **`EntityManagerFactory`** with:

- **`packagesToScan`** for `@Entity` classes
- **`JpaVendorAdapter`** (Hibernate)
- **`JpaProperties`** / `hibernate.*` settings

Boot auto-config hides most of this.

---

## `JpaTransactionManager`

Binds **`EntityManager`** per transaction thread (`@PersistenceContext` injection).

---

## `PersistenceExceptionTranslationPostProcessor`

Turns vendor-specific exceptions into **`DataAccessException`**.

---

## `JpaRepository` (Spring Data)

Not core Framework, but common stack: Spring Data JPA generates queries from method names / `@Query`.

---

## When JDBC vs JPA

| JDBC | JPA |
|------|-----|
| Full SQL control, reporting, bulk | Rich domain model, associations, caching |
| Lighter runtime | More concepts (lazy loading, flush timing) |

---

## Next

- [Spring WebFlux Introduction](26-Spring-WebFlux-Introduction.md)

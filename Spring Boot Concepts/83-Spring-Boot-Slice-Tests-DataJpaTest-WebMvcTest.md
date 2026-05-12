# Slice Tests: `@DataJpaTest` vs `@WebMvcTest` Boundaries

**Slice tests** load a **minimal** Spring context for one layer—faster than **`@SpringBootTest`**, but misuse causes **missing bean** failures or **false confidence**.

---

## `@WebMvcTest`

- Loads **web layer** (controllers, JSON mapping, security slice).
- **Mocks** services with **`@MockBean`** unless importing specific `@Configuration`.

**Good for**: request mapping, validation binding, **security rules** on MVC surface.

---

## `@DataJpaTest`

- Configures **in-memory** or test DB, **JPA** repositories, **`EntityManager`**.

**Good for**: query methods, **custom `@Query`**, cascade rules.

**Not for**: full HTTP stack unless paired with **`@AutoConfigure...`** intentionally.

---

## Pitfalls

- **`@SpringBootTest`** for everything → slow CI; slices need **explicit** collaborators.
- **Flaky** tests when **shared DB** state leaks—prefer **Testcontainers** or **@Transactional rollback** patterns consciously.

---

## Related

- [Spring Boot Testing](13-Spring-Boot-Testing.md)
- [Spring Boot Integration Testing](55-Spring-Boot-Integration-Testing.md)
- [Spring Boot Testcontainers](59-Spring-Boot-Testcontainers.md)

# Spring TestContext Framework

Integration tests load a **reduced** or **full** `ApplicationContext` using **`@ExtendWith(SpringExtension.class)`** (JUnit 5) and **`@ContextConfiguration`** or Boot’s **`@SpringBootTest`**.

---

## `@SpringBootTest`

Loads **full** Boot application (slower) — use **`webEnvironment = RANDOM_PORT`** for servlet integration, **`MOCK`** for `MockMvc`.

---

## Slice tests (Boot)

`@WebMvcTest`, `@DataJpaTest`, `@JsonTest` — load only relevant auto-config for **faster** tests.

---

## `TestRestTemplate` / `WebTestClient`

Hit running server in integration tests.

---

## `@MockBean` / `@SpyBean`

Replace beans in the test context — useful for external systems.

---

## Transaction rollback

`@Transactional` on test class with default rollback keeps DB clean for `@DataJpaTest` style tests.

---

## `@DynamicPropertySource`

Register Testcontainers JDBC URLs into Spring environment before context starts.

---

## Next

- [Spring JDBC and JdbcTemplate](24-Spring-JDBC-JdbcTemplate.md)

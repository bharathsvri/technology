# Spring Data JPA: N+1 Queries and `@EntityGraph`

The **N+1** problem loads **1 parent** then **N additional queries** for each **child** collection—classic JPA performance interview topic.

---

## Symptoms

- Hibernate logs show **many** `SELECT` statements after one **`findAll()`**.
- Latency scales with **row count** linearly.

---

## Mitigations

- **`@EntityGraph`** / **`@NamedEntityGraph`** on repository methods.
- **`JOIN FETCH`** in **`@Query`** (watch **pagination** + **distinct** issues).
- **DTO projections** when graph not needed.

---

## Trade-offs

- **Eager** everywhere → **cartesian product** risk; prefer **fetch plan** per **use case**.

---

## Related

- [Spring Boot Data JPA](09-Spring-Boot-Data-JPA.md)
- [Spring ORM and JPA Integration](../Spring/25-Spring-ORM-and-JPA-Integration.md)

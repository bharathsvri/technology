# `produces` / `consumes` and Content Negotiation

**`@RequestMapping` derivatives** support **`produces`** and **`consumes`** to match **Accept** and **Content-Type** headers—narrowing handler selection and influencing **`406`/`415`** responses.

---

## Examples

```java
@PostMapping(consumes = MediaType.APPLICATION_JSON_VALUE)
@GetMapping(produces = MediaType.APPLICATION_JSON_VALUE)
```

---

## Negotiation interplay

When multiple methods could match, **produces/consumes** constraints participate in **mapping** resolution alongside **path**, **method**, and **params**.

---

## Pitfalls

- **Swagger/OpenAPI** must reflect the same media types or clients mis-generate.
- **Default charset** assumptions for **`text/*`** vs **`application/json`**.

---

## Related

- [HTTP Message Converters](16-HTTP-Message-Converters.md)
- [Spring Boot Content Negotiation](../Spring%20Boot%20Concepts/49-Spring-Boot-Content-Negotiation.md)

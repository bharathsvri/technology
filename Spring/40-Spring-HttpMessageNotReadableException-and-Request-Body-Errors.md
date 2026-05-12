# `HttpMessageNotReadableException` and Request Body Errors

When **`@RequestBody`** deserialization fails (malformed JSON, unknown enum, type mismatch), Spring MVC throws **`HttpMessageNotReadableException`**—map it to **400** with **Problem Details** for consistent APIs.

---

## Common causes

- **Missing** required fields vs **`Optional`** / nullable types.
- **Wrong** `Content-Type` header (`text/plain` vs `application/json`).
- **Date** format not matching **`@JsonFormat`** / Jackson modules.

---

## Handling

**`@ControllerAdvice`** method with **`@ExceptionHandler(HttpMessageNotReadableException.class)`**—log **root cause** (`getMostSpecificCause()`), return **safe** message to clients (no stack traces).

---

## Related

- [Exception Handling in Spring MVC](15-Exception-Handling-in-Spring-MVC.md)
- [HTTP Message Converters](16-HTTP-Message-Converters.md)
- [Spring Boot Problem Details RFC9457](../Spring%20Boot%20Concepts/62-Spring-Boot-Problem-Details-RFC9457.md)

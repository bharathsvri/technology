# Bean Validation: Groups and Custom Constraints

**Jakarta Validation** (`@NotNull`, `@Size`, …) supports **validation groups** so different endpoints validate **different subsets** of the same DTO (e.g. **Create** vs **Update**).

---

## Groups

```java
public interface OnCreate {}
public interface OnUpdate {}

@NotNull(groups = OnCreate.class)
private String name;
```

Controller parameter uses **`@Validated(OnCreate.class)`**.

---

## Custom constraints

Implement **`ConstraintValidator<A, T>`** + composing annotation—centralize **cross-field** rules (password match, date ranges).

---

## Spring MVC integration

**`@Valid`** on **`@RequestBody`** triggers validation; combine with **`MethodArgumentNotValidException`** handler (`ProblemDetails`).

---

## Related

- [Spring Boot Validation](08-Spring-Boot-Validation.md)
- [Validation with Spring](../Spring/17-Validation-with-Spring.md)

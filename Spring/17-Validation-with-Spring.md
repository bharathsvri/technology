# Validation with Spring (Bean Validation)

**Jakarta Bean Validation** (`@NotNull`, `@Size`, `@Email`, …) validates object graphs. Spring integrates it with MVC (`@Valid`), WebFlux, and JPA (`ddl` unrelated—validation is runtime).

---

## DTO validation

```java
public record CreateUser(
    @NotBlank String username,
    @Email String email,
    @Size(min = 8) String password
) {}
```

```java
@PostMapping("/users")
public User create(@Valid @RequestBody CreateUser body) { ... }
```

Failure → **`MethodArgumentNotValidException`** with `BindingResult` field errors.

---

## `@Validated` on class for group validation

```java
@Service
@Validated
public class AccountService {
    public void transfer(@Valid @NotNull TransferRequest r) { ... }
}
```

Enables **method-level** validation (needs `MethodValidationPostProcessor` / Boot auto-config).

---

## Custom constraints

`@Constraint(validatedBy = MyValidator.class)` + `ConstraintValidator<A, T>` implementation.

---

## i18n error messages

`ValidationMessageSource` / `MessageSource` keys in annotation `message = "{key}"`.

---

## Programmatic validation

Inject **`jakarta.validation.Validator`**:

```java
Set<ConstraintViolation<CreateUser>> violations = validator.validate(user);
```

Useful outside web layer.

---

## Next

- [Spring Security Overview](18-Spring-Security-Overview.md)

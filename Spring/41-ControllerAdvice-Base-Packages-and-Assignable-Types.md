# `@ControllerAdvice` Base Packages and Assignable Types

**`@ControllerAdvice`** can limit scope via **`basePackages`**, **`annotations`**, **`assignableTypes`**—prevent **global** exception handlers from swallowing **unrelated** controllers (e.g. **admin** vs **public** API).

---

## Example intent

```java
@ControllerAdvice(basePackageClasses = PublicApi.class)
public class PublicApiExceptionHandler { ... }
```

---

## Ordering

Multiple advice beans: implement **`Ordered`** / **`@Order`** when **overlapping** exception types need deterministic **precedence**.

---

## Related

- [Exception Handling in Spring MVC](15-Exception-Handling-in-Spring-MVC.md)
- [Spring TestContext Framework](23-Spring-Testing-Framework.md)

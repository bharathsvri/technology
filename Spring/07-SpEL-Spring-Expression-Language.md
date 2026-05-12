# Spring Expression Language (SpEL)

**SpEL** is an expression language that evaluates at **runtime** against the Spring **EvaluationContext**—beans, properties, system properties, collections, etc. It is used in **`@Value`**, XML definitions, custom code via `ExpressionParser`, and some integration modules.

---

## `@Value` examples

```java
@Value("${app.name}")           // property placeholder (resolved by Environment)
private String appName;

@Value("#{ systemProperties['user.home'] }")  // SpEL: JVM system property
private String userHome;

@Value("#{ @orderService.defaultLimit }")     // SpEL: call getter on bean named orderService
private int defaultLimit;

@Value("#{ T(java.lang.Math).random() }")     // SpEL: static type access
private double sample;
```

**Note**: `${...}` is a **property placeholder**; `#{...}` is **SpEL**. They can be combined: `@Value("${app.limit:#{10}}")` style patterns exist with careful escaping rules.

---

## Evaluation in code

```java
ExpressionParser parser = new SpelExpressionParser();
StandardEvaluationContext ctx = new StandardEvaluationContext(myRootObject);
int value = parser.parseExpression("price * quantity").getValue(ctx, Integer.class);
```

Useful for **rules engines** or configurable formulas stored in a database (with **extreme care**—see security below).

---

## Common SpEL features

| Feature | Example |
|---------|---------|
| Property navigation | `user.address.city` |
| Safe navigation | `user?.address?.city` (in supported versions / contexts) |
| Collection selection | `orders.?[price > 100]` |
| Projection | `orders.![customer.name]` |
| Ternary | `isVip ? 0.9 : 1.0` |
| Regex match | `name matches '[A-Za-z]+'` |

Exact syntax availability depends on SpEL version and context—consult Spring reference for edge cases.

---

## Security warning

**Never** evaluate SpEL built from **untrusted user input** without a locked-down parser configuration—this is a classic **expression injection** vector (similar to EL injection). For user-defined “rules”, prefer:

- A **restricted DSL** you parse yourself, or
- Sandboxed expression engines with **no bean resolver**, or
- Server-side validation with **fixed** expression templates.

---

## Performance

SpEL expressions are **parsed** and can be **cached**; repeated evaluation on hot paths should reuse parsed `Expression` objects rather than parsing strings in a loop.

---

## Next

- [Aspect-Oriented Programming](08-Aspect-Oriented-Programming.md)

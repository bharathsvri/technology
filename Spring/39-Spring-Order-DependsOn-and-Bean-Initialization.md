# `@Order`, `@DependsOn`, and Bean Initialization Order

Spring **does not** guarantee arbitrary bean creation order unless you declare **dependencies**. **`@DependsOn`** names **other beans** that must be **fully initialized first**; **`@Ordered`** / **`Ordered`** interface orders **components** that participate in **lists** (filters, advices, `ApplicationListener`).

---

## `@DependsOn`

Use when **side-effect initialization** must precede another bean (e.g. **cache warmer** before **consumer**) without a direct **injection** edge.

---

## `@Order` on slices

**`@Order`** on **`WebMvcConfigurer`** beans, **security filters**, **AOP advices**—lower **precedence value** runs **earlier** (Integer.MIN_VALUE first—verify Spring variant docs).

---

## Pitfalls

- **`@Order` on `@Bean` methods** does not order **singleton instantiation** by itself—use **`@DependsOn`** or **constructor injection** for hard deps.

---

## Related

- [Bean Lifecycle and Scopes](05-Bean-Lifecycle-and-Scopes.md)
- [Configuration and Component Scanning](06-Configuration-and-Component-Scanning.md)

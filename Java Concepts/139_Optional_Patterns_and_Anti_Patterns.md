# `Optional` — Patterns and Anti-Patterns

**`Optional<T>`** models **absent** values without **`null`** in APIs—but misused `Optional` fields and **`get()`** without checks are **code smell**.

---

## Prefer

- **Return** `Optional` from service lookups; **never** use as **field** type on entities (JPA).
- **`orElseThrow`**, **`orElse`**, **`ifPresent`**, **`map`/`flatMap`** for pipelines.

---

## Avoid

- **`Optional.ofNullable`** then **`get()`** unconditionally.
- **`Optional` parameters**—overload methods or use **nullable** with **Objects.requireNonNull** at boundary.

---

## Streams

**`findFirst()`** returns `Optional`—chain **declaratively**.

---

## Related

- [Optional Class](27_Optional_Class.md)
- [Streams API](23_Streams_API.md)

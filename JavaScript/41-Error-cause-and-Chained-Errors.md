# `Error.cause` and Chained Error Semantics

**`Error.cause`** attaches a **root** error when wrapping failures—improves **logs** and **debuggability** without losing stack context.

```js
try {
  await doWork();
} catch (e) {
  throw new Error('Work pipeline failed', { cause: e });
}
```

---

## Why interviewers like it

- **Structured** unwrap in logging (`err.cause`) vs string concatenation.
- Aligns with **AggregateError** patterns for **multi-failure** operations.

---

## Platform support

Broadly available in modern runtimes—**polyfill** not usually needed in greenfield web targets.

---

## Related

- [Error Handling](18-Error-Handling.md)
- [Promise allSettled any and AggregateError](37-Promise-allSettled-any-and-AggregateError.md)

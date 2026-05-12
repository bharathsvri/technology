# `Throwable.addSuppressed` and Try-with-Resources

When **primary exception** **E1** occurs in **`try`**, and **close()** throws **E2**, Java **suppresses** **E2** on **E1** so callers see the **root** failure first.

---

## `try-with-resources`

Compiler desugars to **`try { open } finally { close }`** with suppression logic for **AutoCloseable**.

---

## Manual suppression

```java
try {
  throw new IOException("read failed");
} catch (IOException e) {
  e.addSuppressed(new IllegalStateException("cleanup"));
  throw e;
}
```

---

## Interview point

**`getSuppressed()`** returns **array** for logging—surface in **structured logs** for diagnostics.

---

## Related

- [try-with-resources and Suppressed Exceptions](123_Try_with_Resources_and_Suppressed_Exceptions.md)
- [Exception Handling](16_Exception_Handling.md)

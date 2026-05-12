# try-with-resources and Suppressed Exceptions

## try-with-resources (Java 7+)
Any type implementing **`java.lang.AutoCloseable`** (including `Closeable`) can appear in a try-with-resources header. The compiler expands it to a safe close pattern even when exceptions occur.

```java
try (var in = Files.newInputStream(path);
     var reader = new InputStreamReader(in, StandardCharsets.UTF_8)) {
    // use reader
} // close in reverse order of declaration
```

### Rules
- Resources are **closed in reverse declaration order**.
- **`close()`** is still called if construction of a later resource fails (for resources already constructed).

## Suppressed Exceptions
If the `try` block throws **T** and `close()` throws **C**, **T** remains the primary exception thrown to callers, while **C** is **suppressed** and attached via `Throwable.addSuppressed`.

### Inspecting

```java
try {
    // ...
} catch (IOException ex) {
    for (Throwable suppressed : ex.getSuppressed()) {
        // log suppressed
    }
}
```

## Manual Pattern (Avoid When Possible)
Nested `finally` blocks to close resources are error-prone; prefer try-with-resources.

## Related Topics
- `16_Exception_Handling.md`
- `20_File_IO.md`, `48_NIO2.md`

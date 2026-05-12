# CompletableFuture — Composition and Pitfalls

**`CompletableFuture<T>`** expresses **async** pipelines with **thenApply**, **thenCompose**, **allOf**, **anyOf**, and explicit **executor** control—ubiquitous in **non-blocking** services and **parallel** fan-out.

---

## Composition vs nesting

Prefer **`thenCompose`** (flat-map) over **`thenApply` returning CF** to avoid **nested** futures.

```java
CompletableFuture<Result> f =
    fetchId()
        .thenCompose(id -> fetchDetails(id))
        .thenApply(details -> mapToResult(details));
```

---

## Thread pools

- **Default `ForkJoinPool.commonPool()`** may saturate under heavy async—pass **`executor`** for I/O work.
- **Blocking calls inside CF** starve the pool—use **bounded** I/O executors.

---

## Error handling

- **`exceptionally` / `handle` / `whenComplete`** for unified cleanup.
- **`join()`** throws **unchecked** `CompletionException`—wrap cause inspection.

---

## Timeouts (Java 9+)

```java
future.orTimeout(2, TimeUnit.SECONDS);
```

Prefer timeouts on **external** dependencies.

---

## Related

- [Reactive Programming](62_Reactive_Programming.md)
- [Concurrency Patterns](61_Concurrency_Patterns.md)

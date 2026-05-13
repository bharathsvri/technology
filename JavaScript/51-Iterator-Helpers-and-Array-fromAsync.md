# JavaScript: Iterator helpers and `Array.fromAsync`

## Iterator helpers (modern ECMAScript)
The language adds **methods on `Iterator.prototype`** (availability depends on your runtime and compile targets) so you can compose lazy pipelines without materializing giant arrays:

- Conceptual analogues of **`map` / `filter` / `take` / `drop` / `flatMap` / `reduce`** on arbitrary iterators.
- Useful for **streaming** sources (`ReadableStream` async iterators, large query results) when paired with appropriate **backpressure** awareness.

### Practical guidance
- If you target older browsers, **polyfill** or stick to **`for...of`** + manual accumulation.
- Prefer **`toArray()`** only when you know the iterator is **bounded**; infinite iterators + eager materialization will hang or exhaust memory.

## `Array.fromAsync`
- **`Array.fromAsync(asyncIterable)`** resolves an **async iterable** (or array of promises) into a **Promise of an array**.
- Cleaner than manual **`for await...of`** when you truly need all values in memory.

### Pitfalls
- Same boundedness warning: **do not** `fromAsync` an infinite async stream.
- Combine with **`AbortSignal`** at the producer (for example `fetch` streams) so cancellation propagates.

## Related docs
- `17-Iterators-and-Generators.md`
- `16-async-and-await.md`, `39-Top-level-await-in-ES-Modules.md`
- `21-Fetch-API-and-HTTP.md`, `38-AbortController-and-Cancellation-Patterns.md`

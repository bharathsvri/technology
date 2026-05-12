# `defineAsyncComponent` with Loading and Error UI

**`defineAsyncComponent`** wraps **dynamic `import()`** with **loading**, **error**, and **delay** options—pairs naturally with **`<Suspense>`** in Vue 3.

---

## Options

- **`loader`**: `() => import('./Heavy.vue')`
- **`loadingComponent` / `errorComponent`**: placeholders during fetch/failure.
- **`delay`**: minimum time before showing loader (avoids flash).
- **`timeout`**: treat slow loads as error.

---

## Error boundaries

Use **`onErrorCaptured`** or route-level handling for **recoverable** failures; async component **`errorComponent`** for **local** fallback.

---

## Related

- [Teleport and Suspense](17-Teleport-and-Suspense.md)
- [Vue Router Lazy Loading and Code Splitting](31-Vue-Router-Lazy-Loading-and-Code-Splitting.md)

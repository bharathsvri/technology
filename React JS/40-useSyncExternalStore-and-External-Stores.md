# `useSyncExternalStore` for External Stores

**`useSyncExternalStore`** subscribes to **non-React** stores (TanStack Store, Zustand vanilla, browser APIs) with **tearing-safe** reads during **concurrent** rendering—replacement patterns for **`useEffect` + subscribe** hacks.

---

## Signature sketch

```ts
const value = useSyncExternalStore(subscribe, getSnapshot, getServerSnapshot?);
```

**`getServerSnapshot`** required for **SSR** parity.

---

## Interview talking points

- **Why**: concurrent features can **interrupt** renders—external stores need **consistent snapshot** rules.
- **Redux** internals leverage this pattern in modern versions.

---

## Related

- [React 18 Concurrent Features](19-React-18-Concurrent-Features.md)
- [Zustand and Jotai Lightweight State](33-Zustand-and-Jotai-Lightweight-State.md)

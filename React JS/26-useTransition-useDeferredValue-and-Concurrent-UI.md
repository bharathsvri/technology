# `useTransition`, `useDeferredValue`, and Concurrent UI

React 18’s **concurrent rendering** lets you **mark updates** as transitions (interruptible, keep UI responsive) and **defer expensive re-renders** of low-priority subtrees.

---

## `useTransition`

```tsx
const [isPending, startTransition] = useTransition();

function onSearchChange(value: string) {
  setQuery(value); // urgent: input stays snappy
  startTransition(() => {
    setHeavyFilter(value); // non-urgent: may lag behind typing
  });
}
```

**`isPending`**: show spinner on **slow** transition path.

**Pitfall**: putting **urgent** state updates inside `startTransition` can make critical UI feel “late.”

---

## `useDeferredValue`

Mirrors a value but **lags** when updates are fast—good for **memoized child lists** / charts that should not block typing.

```tsx
const deferredQuery = useDeferredValue(query);
const results = useMemo(() => compute(deferredQuery), [deferredQuery]);
```

---

## Interview framing

- **Concurrent features** need a **concurrent root** (React 18 createRoot).
- Compare to **`debounce`**: deferral is **integrated** with React priorities, not only timers.

---

## Related

- [React 18 Concurrent Features](19-React-18-Concurrent-Features.md)
- [Performance Optimization](16-Performance-Optimization.md)

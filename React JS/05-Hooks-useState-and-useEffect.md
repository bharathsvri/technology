# `useState` and `useEffect`

These two hooks cover most **local UI state** and **side effects** in React function components. Understanding their **rules** (especially the dependency array) prevents stale closures, infinite loops, and memory leaks.

---

## `useState`

Returns a **pair**: current state value and a **setter** function.

```tsx
import { useState } from 'react';

export function Toggle() {
  const [open, setOpen] = useState(false);

  return (
    <button type="button" onClick={() => setOpen((v) => !v)}>
      {open ? 'Close' : 'Open'}
    </button>
  );
}
```

### Functional updates

When the next state depends on the previous one, pass a **function** to the setter:

```tsx
setCount((c) => c + 1);
```

This avoids stale reads when multiple updates batch together.

### Batching

React **batches** multiple `setState` calls in the same event handler into **one re-render** (React 18+ also batches in async handlers in many cases).

### Initial state cost

```tsx
const [data, setData] = useState(() => expensiveParse(raw));
```

The **initializer function** runs only once on mount — use when initial value is costly.

---

## `useEffect`

Runs **after paint** (by default) to express side effects: network calls, subscriptions, DOM measurements, logging.

```tsx
useEffect(() => {
  const id = setInterval(() => tick(), 1000);
  return () => clearInterval(id); // cleanup on unmount or before re-run
}, [tick]); // dependency array
```

### Dependency array rules

| Array | Meaning |
|-------|---------|
| **omitted** | Runs after **every** render — almost always a bug for subscriptions/fetch |
| **`[]`** | Run **once** on mount; cleanup on unmount |
| **`[a, b]`** | Re-run when **`a` or `b` changes** (by `Object.is`) |

**ESLint `react-hooks/exhaustive-deps`** flags missing dependencies — treat warnings seriously; either add deps or prove why omission is safe (document with comment).

### Fetch example with cleanup

```tsx
useEffect(() => {
  const ac = new AbortController();
  (async () => {
    const res = await fetch(`/api/items?q=${q}`, { signal: ac.signal });
    if (!res.ok) return;
    setItems(await res.json());
  })();
  return () => ac.abort();
}, [q]);
```

When `q` changes, the previous request is **aborted** to avoid race conditions applying stale results.

---

## Strict Mode (development)

React 18 **Strict Mode** may **mount → unmount → mount** components to detect missing cleanups. Effects run twice on first mount in dev — ensure cleanup reverses **all** subscriptions.

---

## When *not* to use `useEffect`

- **Deriving** state from props/state — use **`useMemo`** or compute during render instead of syncing with an effect.
- **Event handlers** — do work directly in `onClick` when no subscription across renders is needed.

---

## Next

- [useContext and useReducer](06-Hooks-useContext-and-useReducer.md)

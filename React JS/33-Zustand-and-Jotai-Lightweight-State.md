# Lightweight Client State: Zustand and Jotai (Overview)

**Server state** belongs in **TanStack Query**; **UI state** often fits **colocated** `useState` or small global stores.**Zustand** and **Jotai** are popular **minimal** alternatives to Redux for **client-only** state.

---

## Zustand (store model)

- Single **`create((set) => ({ ... }))`** store with **selector** subscriptions.
- **No Provider** required (optional context patterns exist).

**Good for**: session UI flags, **wizard** steps, **shopping cart** client cache.

---

## Jotai (atom model)

- **Fine-grained** atoms compose like **`useState`** in a graph.
- Great when many **independent** knobs should not rerender the whole tree.

---

## Interview comparison

| | Redux Toolkit | Zustand | Jotai |
|--|---------------|---------|-------|
| Boilerplate | Higher, opinionated | Low | Low |
| DevTools | Strong | Plugin | Plugin |
| Mental model | Actions/reducers | Store | Atoms |

---

## Related

- [State Libraries Overview](18-State-Libraries-Overview.md)
- [TanStack Query React Query Patterns](27-TanStack-Query-React-Query-Patterns.md)

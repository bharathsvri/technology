# `useOptimistic` and `useFormStatus` (React 19+)

**`useOptimistic`** updates the UI **immediately** while an async **action** is in flight, rolling back on failure—pairs with **React 19** form **`action`** props and **`useFormStatus`** for **pending** UI on the same form.

---

## Mental model

- **`useOptimistic(state, reducer)`** returns **`[optimisticState, addOptimistic]`**.
- **`useFormStatus()`** reads **`pending`**, **`method`**, **`action`** from **nearest `<form>`** context (when using the new form integration).

---

## Interview framing

- Compare to **manual** “temp row + rollback” patterns in older React.
- **Server** remains source of truth—optimistic UI is **UX**, not **authorization**.

---

## Related

- [React 18 Concurrent Features](19-React-18-Concurrent-Features.md)
- [Forms in React](10-Forms-in-React.md)

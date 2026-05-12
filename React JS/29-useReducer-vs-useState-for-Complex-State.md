# `useReducer` vs `useState` for Complex State

**`useState`** fits independent values; **`useReducer`** shines when updates are **event-driven**, **multi-field**, or need **centralized** transition logic—similar to a lightweight state machine.

---

## When `useReducer` helps

- Form-like state with **many fields** updated together.
- **Next state** depends on **previous state** *and* an **action** label (`ADD_TODO`, `TOGGLE`).
- Easier **testing** of pure **`reducer(state, action)`** functions.

---

## When `useState` stays simpler

- One or two booleans/strings.
- No shared transition rules—**lifting state** may be enough.

---

## Compare to context

**`useReducer` + `useContext`** can replace small Redux-like stores—watch **re-render** scope; **split contexts** or memoize consumers.

---

## Related

- [Hooks useContext and useReducer](06-Hooks-useContext-and-useReducer.md)
- [State Libraries Overview](18-State-Libraries-Overview.md)

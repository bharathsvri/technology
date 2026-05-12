# `React.memo`, `useMemo`, and `useCallback` — When They Help

These APIs **reduce work**—they do **not** fix algorithmic slowness. Interviews want you to separate **referential equality** issues from **real** performance problems.

---

## `React.memo`

Shallow-compares **props**; skip re-render if **equal**.

**Use when**: expensive pure child re-renders often with **same** props from a **frequently updating** parent.

**Avoid when**: props change every render anyway (inline objects/functions without memoization).

---

## `useMemo`

Caches a **computed value** across renders when deps unchanged.

**Use for**: heavy pure derivations, **stable** object identity passed to **`memo`** children.

---

## `useCallback`

Caches **function identity** for **`memo`** children or **effect** dependency lists.

**Pitfall**: empty deps with **stale closure** bugs; over-memoization hurts readability.

---

## Profiling first

Use **React Profiler** before sprinkling **`memo`/`useCallback`**—often **list virtualization** or **fewer state subscriptions** wins more.

---

## Related

- [Hooks useMemo useCallback and useRef](07-Hooks-useMemo-useCallback-and-useRef.md)
- [Performance Optimization](16-Performance-Optimization.md)

# `shallowRef` and `shallowReactive`

Vue’s **deep reactivity** walks nested objects. **`shallowRef`** and **`shallowReactive`** stop at **one level**—mutating inner fields **does not** trigger updates unless you **replace** the root value.

---

## When to use

- **Large** immutable-style models (Three.js objects, charts) where deep proxy cost is high.
- Holding **external** library instances you must not proxy deeply.

---

## `shallowRef` pattern

```ts
const state = shallowRef({ count: 0 });
state.value = { ...state.value, count: state.value.count + 1 }; // trigger
```

---

## Pitfalls

- Easy to **forget** reassignment—subtle “UI not updating” bugs.
- Prefer **`ref` + computed slices** when unsure.

---

## Related

- [Reactivity ref reactive computed](04-Reactivity-ref-reactive-computed.md)
- [Vue Router Lazy Loading and Code Splitting](31-Vue-Router-Lazy-Loading-and-Code-Splitting.md)

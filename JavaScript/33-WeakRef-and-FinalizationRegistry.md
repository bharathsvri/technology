# WeakRef and FinalizationRegistry

**WeakRef** and **FinalizationRegistry** are advanced **memory-conscious** APIs for holding **weak** references to objects and observing **garbage collection** (best-effort). Rare in app code; common in **frameworks**, **caches**, and **native bridge** code.

---

## `WeakRef`

```js
const ref = new WeakRef(largeObject);
const current = ref.deref(); // undefined if collected
```

**Weak**: does **not** prevent GC of the target. Use for **optional caches** (e.g. derived metadata for a DOM node that may disappear).

---

## `FinalizationRegistry`

Register a **cleanup callback** invoked **after** an object is **likely** collected (timing is **not** guaranteed; never rely on it for correctness of business logic).

```js
const registry = new FinalizationRegistry((heldValue) => {
  // e.g. release native handle identified by heldValue
});
registry.register(targetObject, 'token-for-cleanup', heldValue);
```

---

## Interview framing

- **Contrast** with **`WeakMap`/`WeakSet`**: keys must be objects; no enumeration; **`WeakRef`** targets a **single** object with explicit **`deref`**.
- **GC is non-deterministic** in general—finalizers are **hints**, not RAII like C++ destructors.

---

## Related

- [Map, Set, WeakMap, WeakSet](24-Map-Set-WeakMap-WeakSet.md)
- [Performance and Debugging Tips](32-Performance-and-Debugging-Tips.md)

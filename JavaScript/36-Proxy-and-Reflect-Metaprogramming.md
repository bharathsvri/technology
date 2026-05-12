# `Proxy` and `Reflect` — Metaprogramming

**`Proxy`** intercepts operations on objects (get, set, apply, construct). **`Reflect`** provides default implementations of those operations—often used together for **ORM-like models**, **observation**, and **testing doubles**.

---

## Minimal `get` / `set` trap

```js
const target = { count: 0 };
const p = new Proxy(target, {
  set(obj, prop, value) {
    if (prop === 'count' && typeof value !== 'number') throw new TypeError();
    return Reflect.set(obj, prop, value);
  },
});
```

---

## Common traps

| Trap | Use |
|------|-----|
| **`get`** | Virtual properties, logging |
| **`set`** | Validation, immutability helpers |
| **`apply`** | Wrap functions |
| **`has` / `ownKeys`** | Hide properties from `in` / `Object.keys` |

---

## Pitfalls

- **Performance**: hot paths with deep proxy chains can regress vs plain objects.
- **Debugging**: stack traces can be noisier; devtools “inspect” may show proxy wrappers.
- Vue 3 **`reactive()`** uses Proxies—do not **unwrap** assumptions in library interop.

---

## Related

- [Objects, Destructuring, and Spread](13-Objects-Destructuring-and-Spread.md)
- [Design Patterns in JavaScript](29-Design-Patterns-in-JavaScript.md)

# `Symbol.species` and Well-Known Symbols

**Well-known symbols** customize built-in operations: **`Symbol.iterator`**, **`Symbol.toStringTag`**, **`Symbol.species`** (constructor used by **`map`/`slice`** methods on subclasses), etc.

---

## `Symbol.species` example intent

```js
class MyArray extends Array {
  static get [Symbol.species]() { return Array; }
}
```

Ensures **derived** methods return **plain `Array`**, not **`MyArray`**.

---

## Interview scope

- Know **names** and **purpose**; deep customization is rare in app code.
- **`Symbol.asyncIterator`** for **async iterables**.

---

## Related

- [Symbol and BigInt](25-Symbol-and-BigInt.md)
- [Iterators and Generators](17-Iterators-and-Generators.md)

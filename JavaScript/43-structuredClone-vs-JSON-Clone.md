# `structuredClone` vs `JSON.parse(JSON.stringify())`

**`structuredClone`** (modern runtimes) deep-clones many types **`JSON` cannot**: **`Date`**, **`Map`**, **`Set`**, **`ArrayBuffer`**, circular references, etc.

---

## JSON cloning limits

- Drops **`undefined`**, converts **`NaN`** to **`null`**, loses **`Map`** keys object identity.
- **No** circular structure support.

---

## `structuredClone` limits

Still cannot clone **functions**, some **DOM** nodes, or **class** instances with **behavior** unless they are plain data transfer objects.

---

## When to use which

| Need | Prefer |
|------|--------|
| Plain JSON-safe POJO | `JSON` round-trip (simple) |
| Dates, binary, Maps | `structuredClone` |
| Cross-realm / workers | `structuredClone` aligns with `postMessage` |

---

## Related

- [JSON](14-JSON.md)
- [Web Workers and structuredClone](34-Web-Workers-and-structuredClone.md)

# `Promise.allSettled`, `any`, and `AggregateError`

Beyond **`Promise.all` / `race`**, modern interviews expect **`allSettled`** (never rejects on individual failure), **`any`** (first fulfillment wins; **`AggregateError`** if all reject), and knowing **when** to use each.

---

## `Promise.allSettled`

```js
const results = await Promise.allSettled([fetchA(), fetchB()]);
for (const r of results) {
  if (r.status === 'fulfilled') use(r.value);
  else log(r.reason);
}
```

**Use**: dashboards / partial success—**do not fail the whole batch** because one dependency failed.

---

## `Promise.any`

First **fulfilled** promise wins. If **all** reject → **`AggregateError`** with **`errors`** array.

**Use**: redundant endpoints / **multi-region** fallback reads.

---

## `AggregateError`

Represents **multiple** errors (also from **`Promise.any`**). Inspect **`.errors`** in logs.

---

## Contrast

| API | Rejects when | Resolves with |
|-----|----------------|---------------|
| **`all`** | First rejection | Array of values |
| **`allSettled`** | Never (per-entry status) | Status objects |
| **`race`** | First settlement (fulfill or reject) | Single value/reason |
| **`any`** | All reject | First fulfillment |

---

## Related

- [Promises](15-Promises.md)
- [async and await](16-async-and-await.md)

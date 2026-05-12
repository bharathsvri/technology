# Promises

A **Promise** represents a value that may be **available now**, **later**, or **never** (rejected). Promises unify **callback-heavy** APIs and compose with **`then` / `catch` / `finally`**.

---

## States

A promise is **pending**, then settles to **fulfilled** with a value or **rejected** with a reason.

---

## Construction

```js
const p = new Promise((resolve, reject) => {
  setTimeout(() => resolve(42), 100);
});
```

Call **`resolve`** once for success, **`reject`** for failure (prefer **`Error`** objects).

---

## Chaining

`then` returns a **new** promise — enables pipelines.

```js
fetch('/api/user')
  .then((r) => {
    if (!r.ok) throw new Error(r.statusText);
    return r.json();
  })
  .then((user) => user.name)
  .catch((err) => {
    console.error(err);
    return 'Guest';
  })
  .finally(() => console.log('done'));
```

---

## Composition helpers

| API | Use |
|-----|-----|
| **`Promise.all`** | All must succeed; fails fast on first rejection |
| **`Promise.allSettled`** | Wait for all; inspect `{status, value|reason}` per item |
| **`Promise.race`** | First settled wins |
| **`Promise.any`** | First **fulfillment** wins; `AggregateError` if all reject |

---

## Microtask ordering

`then` callbacks run as **microtasks** after the current JS stack clears — order matters with `queueMicrotask` and `await`.

---

## Next

- [async and await](16-async-and-await.md)

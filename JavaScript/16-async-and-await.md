# `async` and `await`

**`async`/`await`** is syntactic sugar over Promises: **`async` functions always return a Promise**, and **`await`** pauses the async function until a Promise settles (without blocking the main thread’s other work in the browser event loop).

---

## Basic pattern

```js
async function loadUser(id) {
  const res = await fetch(`/api/users/${id}`);
  if (!res.ok) {
    throw new Error(`HTTP ${res.status}`);
  }
  return res.json();
}
```

Errors thrown inside `async` functions become **rejected promises** automatically.

---

## Parallelism

Sequential (slower):

```js
const a = await fetchA();
const b = await fetchB();
```

Parallel when independent:

```js
const [a, b] = await Promise.all([fetchA(), fetchB()]);
```

---

## `try` / `catch` / `finally`

```js
async function safe() {
  try {
    return await risky();
  } catch (e) {
    console.error(e);
    return null;
  } finally {
    cleanup();
  }
}
```

---

## Top-level `await` (modules)

Allowed at **module** top-level — useful for bootstrapping apps; blocks module evaluation until done.

---

## Next

- [Iterators and generators](17-Iterators-and-Generators.md)

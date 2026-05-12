# Variables: `let`, `const`, and `var`

Choosing the right declaration affects **scope**, **reassignment rules**, and **temporal dead zone** behavior. Modern code should almost always use **`const` by default**, then **`let`** when rebinding is required, and **`avoid `var`** in new code.

---

## `const`

- **Block-scoped** (visible only inside `{ ... }` where declared).
- The **binding** cannot be reassigned.
- **Object contents** can still mutate (`const user = {}; user.name = 'Ada'` is legal).

```js
const API_BASE = 'https://api.example.com';
// API_BASE = 'x'; // TypeError in strict mode
```

Use `const` for imports, configuration, and most locals—signals “this reference stays fixed”.

---

## `let`

- **Block-scoped**.
- Can be **reassigned** (not redeclared in same block).

```js
for (let i = 0; i < 3; i++) {
  setTimeout(() => console.log(i), 10); // 0,1,2 — fresh binding per iteration
}
```

With **`var`**, the loop variable would be **one** shared binding → classic `3,3,3` timeout bug.

---

## `var` (legacy)

- **Function-scoped**, not block-scoped — leaks out of `if` blocks.
- **Hoisted** to top of function with `undefined` until assigned.
- Allows **redeclaration** in same scope — confusing.

```js
function demo() {
  if (true) {
    var x = 1;
  }
  console.log(x); // 1 — visible outside `if`
}
```

**Rule**: do not use `var` in new code; use `let`/`const`.

---

## Temporal Dead Zone (TDZ)

From the start of the block until the `let`/`const` line executes, the identifier is in the **TDZ** — accessing it throws **`ReferenceError`** (unlike `var` which yields `undefined`).

```js
function f() {
  console.log(x); // ReferenceError in TDZ
  let x = 1;
}
```

---

## Global `let` vs `var`

`let` at top level does **not** create a property on `globalThis` in modules; `var` in non-module scripts did attach to `window` historically.

---

## Next

- [Data types and coercion](03-Data-Types-and-Type-Coercion.md)

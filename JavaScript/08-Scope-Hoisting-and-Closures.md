# Scope, Hoisting, and Closures

**Scope** determines where bindings are visible. **Hoisting** describes when declarations become visible. **Closures** let inner functions retain access to outer variables after the outer function returns.

---

## Lexical scope

JavaScript uses **lexical (static) scoping**: inner functions see variables from **outer blocks** where they were **written**, not where they are **called**.

```js
const x = 10;
function outer() {
  const y = 20;
  return function inner() {
    return x + y;
  };
}
const f = outer();
f(); // 30 — still sees y even after outer returned
```

---

## Closures in practice

- **Module pattern** — hide private state.
- **Event handlers** — capture configuration from setup scope.
- **Debounced functions** — retain timer id across calls.

```js
function makeCounter() {
  let n = 0;
  return () => ++n;
}
const c = makeCounter();
c(); // 1
c(); // 2
```

---

## Hoisting with `var`

`var` declarations are hoisted to the top of their **function** scope and initialized with **`undefined`** until the assignment line runs.

```js
function f() {
  console.log(x); // undefined (not ReferenceError)
  var x = 1;
}
```

---

## `let` / `const` hoisting

They are hoisted to the block but live in the **temporal dead zone** until initialized — accessing them early is **`ReferenceError`**.

---

## Module scope

Each ES module file has its own **top-level scope** — not global pollution.

---

## Next

- [The this keyword](09-The-this-Keyword.md)

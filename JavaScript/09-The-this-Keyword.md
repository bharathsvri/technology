# The `this` Keyword

## Rules of thumb

- **Method call** `obj.method()` → `this` is `obj`.
- **Plain function** in non-strict mode (global) → `this` is global object; strict → `undefined`.
- **`call` / `apply` / `bind`** — set `this` explicitly.

## Arrow functions

Inherit lexical `this` from enclosing scope.

## Classes

`this` in instance methods refers to the instance.

Next: [Prototypes and classes](10-Prototypes-and-Classes.md).

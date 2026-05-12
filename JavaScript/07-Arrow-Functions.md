# Arrow Functions

## Syntax

```js
const double = (x) => x * 2;
const add = (a, b) => a + b;
```

## Lexical `this`

Arrows **inherit** `this` from enclosing scope—no own `this` binding. Good for callbacks; **not** for object methods that need `this` unless wrapped.

## No `arguments` object

Use **rest parameters** instead.

## Not constructable

Cannot use `new` with an arrow function.

Next: [Scope and closures](08-Scope-Hoisting-and-Closures.md).

# Functions and Parameters

Functions are **first-class values**: assign them to variables, pass them as arguments, return them from other functions. That powers **callbacks**, **higher-order array methods**, and **middleware** patterns.

---

## Declarations vs expressions

```js
// Declaration — hoisted as a whole
function add(a, b) {
  return a + b;
}

// Named function expression — handy for stack traces in recursion
const mul = function multiply(a, b) {
  return a * b;
};

// Anonymous function expression
const div = function (a, b) {
  return a / b;
};
```

---

## Default parameters

```js
function greet(name = 'Guest') {
  return `Hello, ${name}`;
}
```

Defaults are evaluated **at call time** when the argument is **missing** (`undefined`), not when `null` is passed unless you normalize.

---

## Rest parameters

Collect remaining arguments into a **real array**:

```js
function sum(...nums) {
  return nums.reduce((a, n) => a + n, 0);
}
sum(1, 2, 3); // 6
```

Contrast with the old **`arguments`** object — rest is always an array and works with arrow functions.

---

## Higher-order functions

Functions that take or return functions:

```js
function withLogging(fn) {
  return (...args) => {
    console.log('call', fn.name, args);
    return fn(...args);
  };
}
```

Array methods `map`, `filter`, `reduce` are built-in higher-order patterns.

---

## `this` in plain functions

Depends on **call site** (object method vs bare call). Covered in detail in [The this keyword](09-The-this-Keyword.md).

---

## Next

- [Arrow functions](07-Arrow-Functions.md)

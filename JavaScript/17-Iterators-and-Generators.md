# Iterators and Generators

## Iterable protocol

Object with `Symbol.iterator` method returning iterator `{ next() }`.

## `for...of`

Consumes iterables (arrays, strings, Maps, Sets, generators).

## Generators

```js
function* range(n) {
  for (let i = 0; i < n; i++) yield i;
}
```

`yield*` delegates to another iterable.

## Use cases

Lazy sequences, redux-saga-style flows, custom async patterns (with care).

Next: [Error handling](18-Error-Handling.md).

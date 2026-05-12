# Objects, Destructuring, and Spread

## Object literals

Shorthand `{ name, age }` when variable names match keys.

## Destructuring

```js
const { id, title = 'Untitled' } = post;
const [first, second] = tuple;
```

## Spread and rest (objects)

```js
const merged = { ...defaults, ...options };
const { password, ...safeUser } = user;
```

## `Object` helpers

`keys`, `values`, `entries`, `assign`, `freeze`, `defineProperty`.

Next: [JSON](14-JSON.md).

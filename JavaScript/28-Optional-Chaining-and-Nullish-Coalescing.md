# Optional Chaining and Nullish Coalescing

## Optional chaining `?.`

Short-circuits on `null` or `undefined`:

```js
user?.address?.zip
arr?.[0]
fn?.()
```

## Nullish coalescing `??`

Falls back only for **`null`/`undefined`**, unlike `||` which treats `0` and `''` as falsy.

```js
const port = config.port ?? 3000;
```

## Combining

`const name = user?.profile?.name ?? 'Anonymous';`

Next: [Design patterns](29-Design-Patterns-in-JavaScript.md).

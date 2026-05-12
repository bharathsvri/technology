# Dynamic `import()`

## Code splitting

Returns a **Promise** of module namespace:

```js
const { init } = await import('./heavy.js');
```

## Conditional loading

Load features only when needed (lazy routes, admin panels).

## Browser and bundlers

Webpack/Vite/Rollup analyze static `import()` for chunk boundaries.

Next: [Performance](32-Performance-and-Debugging-Tips.md).

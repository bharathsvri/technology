# Error Handling

## try / catch / finally

```js
try {
  risky();
} catch (err) {
  console.error(err);
} finally {
  cleanup();
}
```

## Error types

`Error`, `TypeError`, `ReferenceError`, `SyntaxError`, custom subclasses.

## Promise errors

`.catch` on chains; `try/catch` around `await` in `async` functions.

## Global handlers

`window.onerror`, `unhandledrejection` for logging—do not rely on them for control flow.

Next: [DOM](19-DOM-Manipulation.md).

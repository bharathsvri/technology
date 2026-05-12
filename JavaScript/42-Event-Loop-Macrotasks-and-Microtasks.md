# Event Loop: Macrotasks vs Microtasks

The **JavaScript event loop** schedules **call stack** work interleaved with **macrotasks** (timers, I/O callbacks) and **microtasks** (`Promise.then`, `queueMicrotask`).

---

## Order (simplified)

1. Run **one macrotask** to completion.
2. **Drain microtask queue** fully.
3. **Render** (if needed) between turns in browsers.

---

## Implications

- **`Promise.then`** runs **before** the next **`setTimeout`** callback scheduled in the same turn.
- **`await`** splits into **microtasks**—ordering surprises in tests.

---

## Interview classic

```js
console.log('A');
setTimeout(() => console.log('B'), 0);
Promise.resolve().then(() => console.log('C'));
console.log('D');
// A, D, C, B
```

---

## Related

- [Timers and Scheduling](27-Timers-and-Scheduling.md)
- [Promises](15-Promises.md)

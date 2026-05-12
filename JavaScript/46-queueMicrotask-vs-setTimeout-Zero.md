# `queueMicrotask` vs `setTimeout(0)`

**`queueMicrotask(fn)`** schedules **before the next macrotask** and **after** the current JS stack clears—finer than **`setTimeout(0)`**, which waits for the **timer** queue.

---

## Ordering

Microtasks (`Promise.then`, `queueMicrotask`) run **in full** before the browser paints the **next frame**’s macrotask batch (simplified).

---

## When `queueMicrotask` helps

- **Cooperative** scheduling after state mutation but **before** paint.
- Library internals ensuring **ordering** without **`Promise`** allocation.

---

## Abuse

Long microtask chains can **starve** input and **INP**—keep callbacks **tiny**.

---

## Related

- [Event Loop Macrotasks and Microtasks](42-Event-Loop-Macrotasks-and-Microtasks.md)
- [Timers and Scheduling](27-Timers-and-Scheduling.md)

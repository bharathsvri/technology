# Timers and Scheduling

## `setTimeout` / `setInterval`

Return numeric handle; `clearTimeout`, `clearInterval`.

## `queueMicrotask`

Schedules microtask—runs before next macrotask (after current JS stack).

## `requestAnimationFrame`

Sync with display refresh for animations.

## Event loop (mental model)

Call stack → microtasks → render → macrotasks (timers, I/O callbacks)—order matters for performance bugs.

Next: [Optional chaining](28-Optional-Chaining-and-Nullish-Coalescing.md).

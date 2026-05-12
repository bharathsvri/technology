# Web Workers, structuredClone, and Transferables

**Web Workers** run JavaScript on a **background thread** so heavy CPU work does not block the **main thread** (scroll, input, INP). Passing data uses **message channels** and **structured cloning**.

---

## Dedicated worker sketch

```js
// main
const worker = new Worker(new URL('./worker.js', import.meta.url), { type: 'module' });
worker.postMessage({ pixels: imageData });
worker.onmessage = (e) => { /* result */ };
```

---

## `structuredClone`

Deep-clones most structured-cloneable types (objects, arrays, `Map`, `Set`, `ArrayBuffer`, etc.). Used **implicitly** for **`postMessage`** payloads between threads.

**Not** cloned: **functions**, DOM nodes in many cases—check MDN for edge cases.

---

## Transferables (zero-copy move)

```js
worker.postMessage(largeArrayBuffer, [largeArrayBuffer]);
```

After **transfer**, the sender’s buffer is **detached**—avoids copying megabytes.

---

## When to use workers

- Image/audio processing, big CSV parsing, cryptography (where allowed).
- **Not** a substitute for **async I/O** on main thread—network is already non-blocking.

---

## Related

- [Performance and Debugging Tips](32-Performance-and-Debugging-Tips.md)
- [Fetch API and HTTP](21-Fetch-API-and-HTTP.md)

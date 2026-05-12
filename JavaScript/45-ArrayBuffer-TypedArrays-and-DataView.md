# `ArrayBuffer`, Typed Arrays, and `DataView`

**Binary data** in JavaScript uses **`ArrayBuffer`** (raw bytes), **typed arrays** (`Uint8Array`, `Float64Array`, …) as **views** over buffers, and **`DataView`** for **mixed-endian** / unaligned reads at **offsets**.

---

## Typed arrays

Share **same buffer** with **different** element widths via **`byteOffset`** and **`byteLength`**—critical for **WASM**, **WebGL**, **crypto**, and **parsing** wire formats.

---

## `DataView`

```js
const dv = new DataView(buffer);
dv.getUint32(0, true); // little-endian
```

**`true`** = little-endian—explicit control per read.

---

## Pitfalls

- **Detached** buffers after **`postMessage(..., [buffer])`** transfer.
- **Floating** rounding when mixing **Float** and **bit** layouts.

---

## Related

- [Web Workers and structuredClone](34-Web-Workers-and-structuredClone.md)
- [Fetch API and HTTP](21-Fetch-API-and-HTTP.md)

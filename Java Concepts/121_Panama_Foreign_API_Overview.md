# Panama Foreign Function & Memory API (FFM) — Overview

**Project Panama** (finalized as **Foreign Function & Memory API** in modern JDKs) replaces much of JNI + `sun.misc.Unsafe` raw memory access with a **supported** API for calling **native libraries** and working with **off-heap memory**.

## Building Blocks (Names Are Conceptual)
- **`Arena`**: manages lifetimes of memory segments (automatic vs manual close).
- **`MemorySegment`**: contiguous region (on-heap or off-heap) with spatial and temporal bounds checked by the API.
- **`Linker` / `SymbolLookup`**: locate native functions in shared libraries.
- **`FunctionDescriptor` + `MethodHandle`**: describe C signatures and invoke native code.

## Compared to JNI
- **Less boilerplate** for many interop scenarios; no separate C glue compilation for simple calls.
- Stronger **safety** story than unchecked pointer arithmetic—still possible to crash if signatures lie.

## Compared to JNA/JNR
Panama is **JDK-first**; third-party libraries may still be simpler for rapid prototyping until team expertise grows.

## When to Use
- Tight integration with **OS APIs** or **GPU / SIMD** libraries not exposed in pure Java.
- **Zero-copy** buffers interacting with native I/O.

## Operational Notes
- Keep native dependencies **pinned by version** and platform matrix (Windows/Linux/macOS).
- Document **ABI** expectations (struct packing, wchar width).

## Related Topics
- `74_JNI.md` (legacy native interop)
- `48_NIO2.md` (off-heap buffers historically via `ByteBuffer`)

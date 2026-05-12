# `FileChannel`, `transferTo`, and Zero-Copy I/O

**`FileChannel`** bridges **NIO** and files, supporting **memory-mapped** I/O, **`transferTo` / `transferFrom`** for **kernel-bypassing** copies between channels when the OS supports it.

---

## `transferTo` (conceptual benefit)

Copies bytes from this channel to a **writable** channel (e.g. **socket**) with fewer **user/kernel** copies than manual byte buffers—useful for **static file servers** and **HTTP** responses.

---

## Memory-mapped files

**`map(FileChannel.MapMode.READ_ONLY, position, size)`** exposes **`MappedByteBuffer`** for **random access** reads of huge files without explicit `read` loops.

**Pitfalls**: **unmap** is not explicit in older APIs; **file growth** and **concurrent writers** need careful coordination.

---

## When interviewers care

- **High-throughput** file-to-network paths.
- **Log indexing** or **embedded** databases touching OS page cache behavior.

---

## Related

- [NIO.2](48_NIO2.md)
- [File I/O](20_File_IO.md)

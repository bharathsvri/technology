# `Executors` and Virtual Thread Per Task Executor (Java 21+)

**`Executors`** factory methods create **thread pools** for **platform threads**. Java 21 adds **`Executors.newVirtualThreadPerTaskExecutor()`** for **unbounded** virtual-thread-per-task scheduling—ideal for **I/O-heavy** workloads with **blocking** code.

---

## Platform pools

- **`newFixedThreadPool(n)`**: bounded concurrency, **unbounded queue**—watch **backpressure**.
- **`newCachedThreadPool()`**: grows with load—risk **OOM** if tasks **infinite**.

---

## Virtual threads

- **Cheap** tasks; blocking calls **pin** carrier threads briefly—still better than **thousands** of platform threads.
- **Not** a substitute for **CPU-bound** parallelism (`parallelStream` still uses **ForkJoinPool**).

---

## Related

- [Virtual Threads](42_Virtual_Threads.md)
- [Multithreading](21_Multithreading.md)

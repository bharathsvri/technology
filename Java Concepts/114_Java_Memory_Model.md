# Java Memory Model (JMM): Visibility and Ordering

The **Java Memory Model** defines **happens-before** relationships: rules that determine when writes by one thread are visible to reads in another, and how instructions may be reordered.

## Why It Matters
Without a happens-before edge, another thread may see **stale** values, **partially constructed** objects, or surprising reorderings—even on single-core systems due to compiler optimizations.

## Happens-Before (Intuition)
If action **A** happens-before action **B**, then **B** observes **A**’s effects (for fields covered by the model). If there is **no** happens-before link, you have a **data race** on non-volatile fields.

## Common Happens-Before Sources
1. **Program order** within a single thread (as-if-serial semantics per thread).
2. **Monitor unlock → lock** on the same monitor: releasing a `synchronized` block happens-before the next thread acquiring it.
3. **`volatile` write → volatile read** of the *same* variable.
4. **`Thread.start()`** happens-before any statement in the started thread.
5. **Thread termination** (join completion) happens-before statements after `join()` in the joining thread.
6. **Default initialization** of fields happens-before any thread starts using the object (with correct publication).

## `volatile`
- Ensures **visibility** of writes across threads for that variable.
- Establishes **ordering** with respect to other volatile/synchronized edges as defined by the JMM.
- **Not** atomic for compound actions like `count++` (still use `AtomicInteger` or synchronization).

## `synchronized`
Provides **mutual exclusion** and **happens-before** between releases and subsequent acquires of the same lock—both atomicity and visibility for guarded data.

## Safe Publication
Avoid letting other threads see a partially constructed object:
- Use **`final`** fields correctly (immutable objects are easiest to share).
- Avoid leaking `this` from constructors to other threads.
- Prefer **static factory methods** or **private constructor + factory** for tricky initialization.

## `java.util.concurrent`
Higher-level utilities (`ConcurrentHashMap`, `Atomic*`, executors) encode correct visibility patterns—prefer them over hand-rolled double-checked locking.

## Related Topics
- `21_Multithreading.md`
- `35_Advanced_Multithreading.md`
- `61_Concurrency_Patterns.md`

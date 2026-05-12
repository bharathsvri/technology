# `CountDownLatch`, `CyclicBarrier`, and `Phaser`

These **`java.util.concurrent`** synchronizers coordinate **groups of threads** beyond raw **`wait`/`notify`**.

---

## `CountDownLatch`

One-shot: **`await()`** blocks until **`countDown()`** reaches **zero** from any threads.

**Use**: **startup** barriers (“wait until all services registered”), **fan-in** parallel search.

**Cannot reset**.

---

## `CyclicBarrier`

Threads **`await()`** together at a **barrier point**; optionally run a **barrier action** when tripped—then **reopens** for next cycle.

**Use**: **phased** parallel computation (iteration steps).

---

## `Phaser`

Flexible **dynamic** registration/deregistration of parties; more complex API—**simulation** / **fork-join** style pipelines.

---

## Interview contrast

| Tool | Reset | Typical pattern |
|------|-------|-------------------|
| **Latch** | No | One-shot wait |
| **Barrier** | Yes | Fixed crew phases |
| **Phaser** | Dynamic parties | evolving parallelism |

---

## Related

- [Multithreading](21_Multithreading.md)
- [Concurrency Patterns](61_Concurrency_Patterns.md)

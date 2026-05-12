# Monotonic Deque — Sliding Window Maximum

Maintain **candidate indices** in **decreasing** order of values for a sliding window of size **k**—**O(n)** for **max in each window**.

---

## Operations

- **Push** next index: pop from **back** while values **≤** new value (they can never be max while new lives).
- **Pop front** if outside window left edge.

---

## Dual monotone queues

Track **min** and **max** simultaneously for **range** queries on windows.

---

## Related

- [Monotonic Stack](37_Monotonic_Stack.md)
- [Sliding Window](20_Sliding_Window.md)

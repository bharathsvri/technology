# `Comparator` Patterns and `PriorityQueue`

**`PriorityQueue`** is a **binary heap** implementation of **`Queue`** ordered by **natural order** or a **`Comparator`**. Interviews combine it with **scheduling**, **merge k lists**, and **Dijkstra**.

---

## Safe `Comparator` contract

Return **negative/zero/positive** for **less/equal/greater**—avoid **integer overflow** in **`a - b`** tricks; use **`Integer.compare(a,b)`**.

**Must be consistent with equals?** Not strictly required, but **equal** elements should compare **0** to avoid PQ surprises.

---

## Mutating elements already in the heap

**Do not** change sort keys without **remove + add**—heap property breaks silently.

---

## Common pattern

```java
Queue<Node> pq = new PriorityQueue<>(Comparator.comparingInt(n -> n.dist));
```

---

## Related

- [Comparable Comparator](55_Comparable_Comparator.md)
- [Collections Framework](17_Collections_Framework.md)
- [Heaps](../Data%20structure%20concepts/09_Heaps.md)

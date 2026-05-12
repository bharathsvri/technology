# Euler Tour and Subtree Queries on Trees

An **Euler tour** (DFS order listing nodes with **repetitions**) flattens a tree to an **array** where **subtrees** become **contiguous intervals**—enables **range queries** with a **Fenwick** or **segment tree**.

---

## Idea

Run DFS; each time you **enter** a node, append its id; **leave** optional depending on variant. Subtree of **u** corresponds to **interval [tin[u], tout[u]]** in timer DFS (alternative to full Euler list).

---

## Applications

- **Subtree sum / min** with point updates.
- **LCA** preprocessing with **RMQ** on Euler **depth** array.

---

## Complexity

**O(n log n)** or **O(n)** preprocessing depending on structure; queries **O(log n)**.

---

## Related

- [Trees](05_Trees.md)
- [Fenwick Tree (BIT)](27_Fenwick_Tree_BIT.md)
- [Segment Tree](26_Segment_Tree.md)

# Centroid Decomposition (Tree Decomposition Sketch)

**Centroid decomposition** recursively removes a **centroid** (node whose subtree removal splits the tree into components of size **≤ n/2**)—a classic **advanced** technique for **path / distance** queries on trees in **O(n log n)** preprocessing.

---

## Centroid definition

For rooted trees, a **centroid** is a node such that **every** subtree after removal has size **at most ⌊n/2⌋**.

At least one centroid always exists; finding it is **DFS size** + **check** in **O(n)** per level.

---

## Algorithm sketch

1. Find centroid of current component.
2. **Solve** contributions involving paths through this centroid (often **count pairs** with distance constraint).
3. **Recurse** on each connected component after removing centroid.

**Depth**: **O(log n)** levels on balanced splits → total often **O(n log n)**.

---

## Typical problems

- Count pairs with distance **≤ K**.
- Update distances with **offline** queries (advanced variants).

---

## Related

- [Trees](05_Trees.md)
- [Advanced Graph Algorithms](28_Advanced_Graph_Algorithms.md)
- [Advanced DP Patterns](30_Advanced_DP_Patterns.md)

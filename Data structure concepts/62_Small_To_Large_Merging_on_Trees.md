# Small-to-Large Merging on Trees (DSU on Tree Sketch)

**Small-to-large** merges **child** frequency maps into the **largest** child’s map to answer **subtree** queries offline in **O(n log n)** total for many problems (e.g. count distinct colors per subtree).

---

## Idea

DFS; keep **`cnt` map** for **heavy child**; for each **light child**, **merge** its counts into heavy’s map by **iterating smaller** into larger (**swap** if needed).

---

## Amortized intuition

Each element moves **O(log n)** times as its map doubles in size—similar to **DSU** small-to-large proofs.

---

## Related

- [Centroid Decomposition Sketch](50_Centroid_Decomposition_Sketch.md)
- [Union-Find (DSU)](24_Union_Find_Disjoint_Set.md)

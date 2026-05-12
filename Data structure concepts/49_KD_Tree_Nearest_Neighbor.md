# k-d Tree — Nearest Neighbor Sketch

A **k-d tree** partitions **k-dimensional space** with axis-aligned hyperplanes—useful for **nearest neighbor** queries in **moderate** dimensions and small/medium **n** (high dimension curse still applies).

---

## Construction

Cycle through axes **x, y, z, …**: choose **median** point on current axis as node, recurse left/right by **splitting plane**.

**Goal**: **balanced** tree → **O(log n)** depth on random-ish data.

---

## Query (nearest neighbor)

Recursive search: at each node, decide which **child** is closer to query point; still may need to **check other subtree** if **distance to splitting plane** is within **current best** radius.

**Average** often **O(log n)**; **worst** can degrade with **bad data distribution**.

---

## Limitations

- **Curse of dimensionality**: in high **k**, performance approaches brute force.
- For **geo** apps, **R-tree** / specialized indexes often win in production systems.

---

## Related

- [Advanced Graph Algorithms](28_Advanced_Graph_Algorithms.md) (spatial is adjacent topic)
- [Divide and Conquer](18_Divide_and_Conquer.md)

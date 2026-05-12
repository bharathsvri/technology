# Closest Pair of Points — Divide and Conquer

Given **n** points in the plane, find the **minimum Euclidean distance** between any pair in **O(n log n)** using **sort + strip merge** (classic D&C interview capstone).

---

## Algorithm sketch

1. **Sort** by **x** coordinate.
2. **Split** plane at median **x**; recursively solve **left** and **right** halves → **`d`** = min of their best distances.
3. **Merge**: only pairs **within `d`** of the **split line** need checking—sort strip by **y**, **sliding window** inner loop (**O(n)** per level in typical analysis).

---

## Why not brute force

**O(n²)** is too slow for **large n**; D&C exploits **geometry** to prune pairs.

---

## Extension awareness

**3D** variants exist; **kd-tree** approximations help in **nearest neighbor** structures for higher dimensions with caveats.

---

## Related

- [Divide and Conquer](18_Divide_and_Conquer.md)
- [k-d Tree](49_KD_Tree_Nearest_Neighbor.md)

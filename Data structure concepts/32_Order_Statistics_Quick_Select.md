# Order Statistics — Quickselect (k-th Smallest)

**Problem**: Given an unsorted array, find the **k-th smallest** element (1-based or 0-based by convention). **Naive**: sort **O(n log n)** and index. **Quickselect** (Hoare’s selection): **expected O(n)**, **worst O(n²)** without randomized pivot.

---

## Core idea (Lomuto-style outline)

1. Pick a **pivot** (random index reduces adversarial worst case).
2. **Partition** so elements `< pivot` are left, `≥` pivot right; pivot lands at index **`p`**.
3. If **`p == k-1`**, return `a[p]`.
4. If **`k-1 < p`**, recurse **left** only.
5. Else recurse **right** for **adjusted k**.

Only **one** branch recurses → expected linear work (geometrically shrinking subproblem sizes).

---

## Complexity

| | Time | Space |
|---|------|--------|
| **Average** | **O(n)** | **O(log n)** stack if random pivots balance; **O(n)** worst stack if always skewed |
| **Worst** | **O(n²)** | bad pivots every time |

### Deterministic **O(n)** median-of-medians

Select pivot using **median of medians** — rarely required in interviews except conceptually.

---

## vs Heap for “top K”

| Approach | Best when |
|----------|-----------|
| **Quickselect** | Single **k** on an **array in memory** |
| **Min-heap size K** | **Streaming** n huge, want **largest K** |
| **Max-heap size K** | **Smallest K** |

---

## Interview pitfalls

- Off-by-one on **k** vs 0-based index.
- Integer overflow in **`mid = (lo + hi + 1) / 2`** variants for lower/upper bound binary search patterns (different problem but same care).

---

## Related

- `13_Sorting_Algorithms.md`, `14_Searching_Algorithms.md`, `09_Heaps.md`

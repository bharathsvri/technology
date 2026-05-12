# Order Statistics and Quickselect

## Problem
Find the **k-th smallest** element in an unsorted array (1-indexed or 0-indexed by convention).

Naive approach: sort in **O(n log n)** and pick index **k-1**.

**Quickselect** achieves **average O(n)** time by borrowing partitioning from quicksort without recursing on both sides.

## Core Idea (Lomuto-style Outline)
1. Pick a **pivot** (randomized pivot reduces adversarial inputs).
2. **Partition** so elements `< pivot` are left, `≥` pivot right.
3. Let `p` be the pivot’s final index:
   - If `p == k-1`, return `a[p]`.
   - If `k-1 < p`, recurse left subarray.
   - Else recurse right subarray for adjusted **k**.

## Complexity
- **Average time**: **O(n)** (geometrically shrinking subproblems).
- **Worst time**: **O(n²)** with bad pivots—**randomize** or use median-of-medians pivot selection for **worst-case O(n)** (more complex, rare in interviews except conceptually).

## Variants
- **Quickselect on a copy** when mutating input is disallowed.
- **Partial sort / `nth_element`** in C++; Java has `Arrays.sort` full sort—implement quickselect manually or use a **heap** of size **k** for streaming top-k.

## Related: Quickselect vs Heap for Top-K
- **Quickselect**: **O(n)** average for single order statistic on materialized array.
- **Min-heap of size k**: **O(n log k)** for **largest k** elements; better when **n** is huge streaming.

## Related Topics
- `13_Sorting_Algorithms.md` (partitioning)
- `14_Searching_Algorithms.md`
- `09_Heaps.md`

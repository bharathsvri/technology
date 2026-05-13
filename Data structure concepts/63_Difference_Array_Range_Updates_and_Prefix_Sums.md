# Difference array: range updates with prefix reconstruction

## Problem
You have an array `A[0..n-1]` and many operations **`add(delta, L, R)`** that must add `delta` to every index in `[L, R]` inclusive. Naive updates cost **O(n)** each.

## Idea
Maintain a **difference array** `D` of length `n + 1` (sentinel optional) such that reconstructing the current values is a **prefix sum** over `D`.

### Apply range add
To add `delta` on `[L, R]`:

- `D[L]   += delta`
- `D[R+1] -= delta` (if `R + 1 < n`)

### Reconstruct current value at index `i`
`A[i] = sum_{k=0..i} D[k]` after all updates.

Point queries are **O(1)** amortized with a running prefix while scanning, or **O(log n)** if you pair the structure with a Fenwick tree when updates become more general.

## Complexity
- Each range update: **O(1)**.
- Full array materialization after `m` updates: **O(n + m)** with one pass.

## When it breaks down
If you need **range sum queries** on the evolving array with interleaved arbitrary updates, graduate to **Fenwick** or **segment tree** (`27_Fenwick_Tree_BIT.md`, `26_Segment_Tree.md`).

## Related docs
- `20_Sliding_Window.md` — complementary linear techniques
- `31_Amortized_Analysis_and_Master_Theorem.md` — reasoning about aggregate costs

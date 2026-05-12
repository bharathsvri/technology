# Sqrt Decomposition on Arrays

**Sqrt decomposition** splits an array into **~√n blocks**, maintaining **block aggregates**—answers **range queries** and **updates** in **O(√n)** with simpler code than **segment trees** for some problems.

---

## Classic: range sum + point update

- Precompute **sum per block**.
- Query **[L,R]`**: **partial edges** brute force + **full blocks** from aggregate.
- **Point update**: adjust element and **block sum**.

---

## vs segment tree

- **Simpler** when constraints allow **O(√n)** (e.g. **n ≤ 10^5**, **q** moderate).
- **Mo's algorithm** on queries is related **sqrt bucketing** technique.

---

## Related

- [Segment Tree](26_Segment_Tree.md)
- [Fenwick Tree (BIT)](27_Fenwick_Tree_BIT.md)

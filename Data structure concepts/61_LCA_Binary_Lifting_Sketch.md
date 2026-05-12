# LCA — Binary Lifting (Sketch)

**Lowest common ancestor (LCA)** queries on a **static tree** can be answered in **O(log n)** after **O(n log n)** preprocessing by **jump pointers** (**binary lifting**).

---

## Preprocess

For each node **u**, store **`up[u][k]`** = **2^k-th** ancestor toward root.

---

## Query

Lift the deeper node until depths match, then lift both from **largest k** downward until parents match.

---

## vs Euler + RMQ

Euler tour + **RMQ** is another LCA route—**binary lifting** is simpler to code in interviews.

---

## Related

- [Euler Tour and Subtree Queries](57_Euler_Tour_and_Subtree_Queries.md)
- [Trees](05_Trees.md)

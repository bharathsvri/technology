# Treap (Cartesian Tree)

A **treap** combines **BST** ordering on keys with **heap** priority on **randomly assigned priorities**—expected **O(log n)** insert/delete/split/merge without explicit AVL rotations.

---

## Invariant

- **BST property** on **key**.
- **Heap property** on **priority** (usually min-heap: parent priority **≤** children).

Random priorities keep tree **balanced** with high probability.

---

## Operations

- **Insert**: attach as leaf, **rotate** until heap property restored (or use implicit treap with **size** field).
- **Split / merge** variants support **rope**-like structures and **range queries** in competitive programming.

---

## vs AVL / Red-Black

- **Simpler code** in contests; **expected** not **worst-case** guarantees.
- Production Java **`TreeMap`** is red-black, not treap—but **concept** matters for interviews.

---

## Related

- [Binary Search Trees](06_Binary_Search_Trees.md)
- [AVL Tree](25_AVL_Tree.md)
- [Skip List](47_Skip_List.md)

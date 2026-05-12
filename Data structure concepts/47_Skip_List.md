# Skip List — Probabilistic Ordered Map

A **skip list** implements **sorted map** operations in **expected O(log n)** using **multiple linked list levels** with **randomized promotion**—simpler than balanced BSTs for many concurrent libraries (**Java `ConcurrentSkipListMap`**).

---

## Structure

- Base level **L0** is a sorted linked list of all keys.
- Higher levels are **sparse express lanes**; each node appears in level **i+1** with probability **p** (often **½**).

---

## Search

Start at **top left**, move **right** while next key **≤ target**, else **down** one level—similar to binary search on a linked structure.

---

## Insert / delete

Search position, **splice** into **L0**, then **coin-flip** to decide how many upper levels to promote—capping at a **max level**.

---

## Expected complexity

- **Search/insert/delete**: **O(log n)** expected.
- **Worst case** **O(n)** if randomness is pathological—rare with good RNG and caps.

---

## vs balanced BST / B+ tree

- **Simpler** implementation, **no rotations**.
- **Cache performance** may lose to **packed array** structures on some workloads.

---

## Related

- [Binary Search Trees](06_Binary_Search_Trees.md)
- [AVL Tree](25_AVL_Tree.md)

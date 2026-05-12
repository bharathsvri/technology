# B-Trees and B+ Trees

**B-trees** generalize binary search trees to **high fanout** nodes so tree **height** stays small on disk—critical when each random read costs milliseconds.

---

## B-tree (conceptual rules)

Parameters vary by textbook (**minimum degree t** or **order**). Common properties:

- All leaves at **same depth** (balanced).
- Each node holds **sorted keys** and **child pointers** between key ranges.
- Nodes are kept **sufficiently full** (except root) via **split/merge** on insert/delete.

### Why high fanout?

If each node holds **hundreds** of keys, height for **billions** of keys is only **2–4** levels → fewer **disk seeks** than a binary tree.

---

## B+ tree (what databases use for clustered indexes)

Differences from many B-tree pedagogical definitions:

- **All keys** (or duplicates) live in **sorted leaf pages** linked as a **linked list** for **range scans**.
- **Internal nodes** store only **separator keys** to guide search → **more fanout**, **shallower** tree.
- **Range query**: find start leaf, scan forward along leaf chain.

---

## Hash index vs B+ tree

| | Point lookup | Range / ORDER BY |
|--|--------------|------------------|
| **Hash** | Often O(1) average | Poor |
| **B+ tree** | O(log n) seeks | Excellent |

---

## Interview phrases

- “**Clustered index** leaf level *is* the table row order in SQL Server.”
- “**Covering index** includes extra columns to avoid **key lookups**.”

---

## Related

- `05_Trees.md`, `06_Binary_Search_Trees.md`, `25_AVL_Tree.md`, `../DBMS/07_Indexes.md`

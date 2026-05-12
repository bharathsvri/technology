# B-Trees and B+ Trees (Database-Oriented Trees)

## Why They Exist
Binary search trees keep height low with **O(log n)** comparisons, but disk-based systems need **high fanout** to minimize **I/O operations**. **B-Trees** generalize balanced search trees so each node holds **many keys** and **many children**.

## B-Tree (Conceptual Rules)
Parameters **t** (minimum degree) or **order** definitions vary by textbook—common properties:
- All leaves appear at the **same depth** (balanced).
- Each node contains **sorted keys** and **pointers** to child subtrees separating key ranges.
- Nodes are kept **partially full** (except root) via split/merge rules on insert/delete.

### Complexity
Search/insert/delete: **O(log n)** with base proportional to **fanout**—shallow trees mean **few disk reads**.

## B+ Tree (What Most RDBMS Indexes Use)
Differences from classic B-tree presentations:
- **All keys** (or duplicates of keys) live in **sorted leaf pages** linked as a **linked list** for fast **range scans**.
- **Internal nodes** store only **separator keys** to guide search—more fanout, shallower tree.
- Range queries: walk down to start leaf, then **scan forward** along leaf chain.

## Why B+ for SQL Server / RDBMS?
- **Range scans** (`BETWEEN`, ordered `GROUP BY`) are common in reporting and joins.
- Larger fanout reduces **tree height** compared to binary structures at disk page sizes (often 8 KB).

## Relationship to Hash Indexes
- **Hash**: great for **point equality** lookups; weak for ranges.
- **B+**: great for **ranges** and **ordered** access paths.

## Related Topics
- `05_Trees.md`, `06_Binary_Search_Trees.md`, `25_AVL_Tree.md`
- `07_Indexes.md` (SQL Server)
- `15_Table_Partitioning.md` (index + partition interplay)

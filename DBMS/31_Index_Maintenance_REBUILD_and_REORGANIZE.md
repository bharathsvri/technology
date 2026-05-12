# Index Maintenance: `REBUILD` vs `REORGANIZE`

Indexes **fragment** over time (page splits, deletes). **Maintenance windows** use **`ALTER INDEX ... REBUILD`** or **`REORGANIZE`** depending on **fragmentation**, **edition**, and **online** requirements.

---

## `REORGANIZE`

- **Lightweight**, **online**, works **page-by-page**; good for **moderate** fragmentation.
- Does not update **statistics** by itself (consider **`UPDATE STATISTICS`**).

---

## `REBUILD`

- **Recreates** the index; can be **offline** or **online** (Enterprise features historically—verify edition).
- Strong fix for **high** fragmentation; resets certain structural issues.

---

## Rule of thumb (starting point)

Many DBAs use thresholds such as **>5–10%** pages for **reorganize**, **>30%** for **rebuild**—**always validate** with your org’s playbook and **`sys.dm_db_index_physical_stats`**.

---

## Related

- [Indexes](07_Indexes.md)
- [Execution Plans and Query Store](17_Execution_Plans_and_Query_Store.md)

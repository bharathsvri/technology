# Covering Indexes and `INCLUDE` Columns

A **covering index** contains **all columns** needed by a query (either as **key** columns or **`INCLUDE`d** non-key columns at the **leaf**), enabling a **seek + lookup-free** plan.

---

## Key vs `INCLUDE`

- **Key columns**: ordered, participate in **seek** and **sort** satisfaction.
- **`INCLUDE` columns**: stored only at **leaf**—good for **wide** select lists that should not bloat **intermediate** B-tree levels.

---

## Trade-offs

- **Wider** indexes hurt **INSERT/UPDATE/DELETE** maintenance and **buffer pool** churn.
- **Too many** indexes → optimizer **choice** overhead.

---

## Interview phrase

“**Key lookup** eliminated because the index **covers** the query.”

---

## Related

- [Indexes](07_Indexes.md)
- [Execution Plans and Query Store](17_Execution_Plans_and_Query_Store.md)

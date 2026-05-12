# `IDENTITY` vs `SEQUENCE` for Surrogate Keys

SQL Server supports **`IDENTITY`** columns and **`SEQUENCE`** objects for **surrogate keys** and **allocation**—trade-offs involve **gaps**, **bulk insert**, and **portability**.

---

## `IDENTITY`

- Simple declaration on column; **automatic** per-row assignment.
- **Gaps** appear after rollbacks, deletes, failed inserts—usually acceptable.

---

## `SEQUENCE`

- **Reusable** across tables, explicit **`NEXT VALUE FOR`**.
- Useful when **reserving** blocks of ids or **sharding** schemes need coordination.

---

## Interview note

**GUID** clustered keys vs **ever-increasing** integers—**fragmentation** vs **distributed** uniqueness trade-off.

---

## Related

- [Constraints](08_Constraints.md)
- [Heap vs Clustered Index](30_Heap_vs_Clustered_Index_Structure.md)

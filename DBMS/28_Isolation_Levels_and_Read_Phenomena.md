# Isolation Levels and Read Phenomena

**Isolation** defines what **concurrent** transactions can observe about each other’s in-flight work. SQL Server implements levels with **locks** and **row versioning** (`READ_COMMITTED_SNAPSHOT`).

---

## Classic phenomena

| Phenomenon | Description |
|------------|-------------|
| **Dirty read** | Read uncommitted data from another transaction |
| **Non-repeatable read** | Same row read twice returns different values after another txn committed |
| **Phantom read** | Range query count changes because another txn **inserted** rows |

---

## SQL Server isolation levels (summary)

- **`READ UNCOMMITTED`**: allows dirty reads (avoid in OLTP correctness paths).
- **`READ COMMITTED`** (default): no dirty reads; behavior differs with **snapshot** options.
- **`REPEATABLE READ`**: locks prevent non-repeatable reads; phantoms still possible.
- **`SERIALIZABLE`**: strongest—range locks reduce phantoms; higher blocking risk.
- **`SNAPSHOT`**: statement-level **MVCC** consistency using **tempdb** version store.

---

## Interview talking points

- **Higher isolation** ↔ **more blocking / deadlocks**; tune queries before blindly raising isolation.
- **`READ_COMMITTED_SNAPSHOT`** ON at database level changes **read** behavior under the hood—know your **DBA standard**.

---

## Related

- [Transactions](09_Transactions.md)
- [Locking, Blocking, Deadlocks](19_Locking_Blocking_Deadlocks.md)

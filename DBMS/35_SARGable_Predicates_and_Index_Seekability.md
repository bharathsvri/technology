# SARGable Predicates and Index Seekability

A predicate is **SARGable** (**Search ARGument able**) if SQL Server can use a **B-tree index seek** on a column without wrapping that column in **non-deterministic** or **value-changing** functions on **every row**.

---

## Common non-SARGable patterns

- **`WHERE YEAR(OrderDate) = 2024`** → wrap column; prefer **range**:

```sql
WHERE OrderDate >= '2024-01-01' AND OrderDate < '2025-01-01'
```

- **`WHERE LOWER(Email) = @e`** → prevents simple seek on **`Email`** unless persisted computed column / index.

---

## Implicit conversions

Compare **matching types** (`varchar` vs **`nvarchar`**, **`int` vs `bigint`**) to avoid scans caused by **conversion** on the column side.

---

## Interview phrase

“Keep the **indexed column** **clean** on the **left-hand** side of predicates.”

---

## Related

- [Indexes](07_Indexes.md)
- [Execution Plans and Query Store](17_Execution_Plans_and_Query_Store.md)

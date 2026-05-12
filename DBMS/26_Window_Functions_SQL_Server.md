# Window Functions in SQL Server

**Window functions** compute a value **per row** using a **window** of related rows **without collapsing** the result set (unlike plain **`GROUP BY`**).

---

## Core syntax

```sql
SELECT
  OrderId,
  CustomerId,
  OrderTotal,
  SUM(OrderTotal) OVER (PARTITION BY CustomerId) AS CustomerGrandTotal,
  ROW_NUMBER() OVER (PARTITION BY CustomerId ORDER BY OrderDate DESC) AS RecentRank
FROM Sales.Orders;
```

---

## Common functions

| Function | Typical use |
|----------|-------------|
| **`ROW_NUMBER`** | Dedup “latest row per key” in a CTE |
| **`RANK` / `DENSE_RANK`** | Leaderboards with ties |
| **`LAG` / `LEAD`** | Prior/next period comparisons |
| **`SUM(...) OVER`** | Running totals |

---

## Performance

- Needs sensible **index** on **`PARTITION BY` + `ORDER BY`** columns for large scans.
- Compare to **correlated subqueries**—optimizers often window better with modern CE.

---

## Related

- [Queries](03_Queries.md)
- [Execution Plans and Query Store](17_Execution_Plans_and_Query_Store.md)

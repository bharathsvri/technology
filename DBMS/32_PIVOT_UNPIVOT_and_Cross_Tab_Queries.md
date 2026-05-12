# `PIVOT`, `UNPIVOT`, and Cross-Tab Queries

**PIVOT** turns **row values** into **column headers** (cross-tab); **UNPIVOT** does the inverse—common in **reporting** and **Excel-like** exports in SQL Server.

---

## `PIVOT` sketch

```sql
SELECT *
FROM (
  SELECT Year, Quarter, Amount FROM dbo.Sales
) src
PIVOT (SUM(Amount) FOR Quarter IN ([Q1],[Q2],[Q3],[Q4])) pvt;
```

**Note**: column list **`IN (...)`** is **static** unless built with **dynamic SQL** (`12_Dynamic_SQL.md`).

---

## `UNPIVOT`

Normalizes wide tables back to **tidy** rows for ETL or analytics.

---

## Alternatives

- **`CASE` + `GROUP BY`** is often clearer for simple pivots.
- **Window functions** (`26_Window_Functions_SQL_Server.md`) for running comparisons without reshaping.

---

## Related

- [Queries](03_Queries.md)
- [Window Functions](26_Window_Functions_SQL_Server.md)

# Common Table Expressions (CTEs) and Recursion

A **CTE** (`WITH` clause) names a subquery for readability, reuse in one statement, and supports **recursive** queries for **hierarchies**.

---

## Non-recursive CTE

```sql
WITH Recent AS (
  SELECT * FROM dbo.Orders WHERE OrderDate >= DATEADD(day, -30, SYSUTCDATETIME())
)
SELECT CustomerId, COUNT(*) FROM Recent GROUP BY CustomerId;
```

**Interview note**: CTEs are **optimization fences** in some plans—always **verify** plans vs inlined subqueries on hot paths.

---

## Recursive CTE (org chart / BOM)

```sql
WITH Org AS (
  SELECT EmployeeId, ManagerId, FullName, 0 AS Level
  FROM dbo.Employees WHERE ManagerId IS NULL
  UNION ALL
  SELECT e.EmployeeId, e.ManagerId, e.FullName, o.Level + 1
  FROM dbo.Employees e
  INNER JOIN Org o ON e.ManagerId = o.EmployeeId
)
SELECT * FROM Org OPTION (MAXRECURSION 200);
```

Always set **`MAXRECURSION`** guard to prevent runaway cycles.

---

## Related

- [Queries](03_Queries.md)
- [Dynamic SQL](12_Dynamic_SQL.md)

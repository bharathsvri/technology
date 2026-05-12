# `STRING_AGG` and Ordered Concatenation

**`STRING_AGG`** concatenates string values **per group** with a **separator**—SQL Server 2017+. **`WITHIN GROUP (ORDER BY ...)`** controls **concat order**.

```sql
SELECT DepartmentId,
       STRING_AGG(EmployeeName, ', ') WITHIN GROUP (ORDER BY EmployeeName) AS Names
FROM HR.Employees
GROUP BY DepartmentId;
```

---

## Limits

**Output length** can hit **8k** / **LOB** boundaries—watch **very large** groups; consider **`FOR XML PATH`** legacy patterns for edge cases.

---

## Related

- [Queries](03_Queries.md)
- [Window Functions](26_Window_Functions_SQL_Server.md)

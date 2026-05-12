# `OUTER APPLY` vs `LEFT JOIN` + `TOP`

**`OUTER APPLY`** correlates each left row with a **right-hand subquery** that can reference **left columns**—like a **lateral join**. It often replaces **correlated subqueries** and can be clearer than **`LEFT JOIN` + `GROUP BY`** patterns for **per-row top-N**.

---

## Pattern: latest order per customer

```sql
SELECT c.CustomerId, o.*
FROM Sales.Customers c
OUTER APPLY (
  SELECT TOP (1) *
  FROM Sales.Orders o
  WHERE o.CustomerId = c.CustomerId
  ORDER BY o.OrderDate DESC
) o;
```

**`CROSS APPLY`** requires a match; **`OUTER APPLY`** preserves customers with **no** orders.

---

## vs window functions

**`ROW_NUMBER() OVER (PARTITION BY ...)`** often competes—optimizer may choose similar plans; pick **readability**.

---

## Related

- [Queries](03_Queries.md)
- [Window Functions](26_Window_Functions_SQL_Server.md)

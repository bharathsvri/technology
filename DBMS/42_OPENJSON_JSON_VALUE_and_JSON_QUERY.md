# `OPENJSON`, `JSON_VALUE`, and `JSON_QUERY`

SQL Server **JSON functions** parse **NVARCHAR JSON** into relational shapes: **`OPENJSON`** for **tables**, **`JSON_VALUE`** for **scalars**, **`JSON_QUERY`** for **objects/arrays**.

---

## `OPENJSON` with explicit schema

```sql
SELECT *
FROM OPENJSON(@json)
WITH (
  id INT '$.id',
  name NVARCHAR(100) '$.name'
);
```

---

## Indexing JSON

Use **computed columns** + **index** on **`JSON_VALUE`** paths for hot filters—mind **KEYLIST** cardinality.

---

## vs relational modeling

**Normalize** stable entities; JSON columns for **variable** payloads and **integration** edges.

---

## Related

- [JSON in SQL Server](16_JSON_in_SQL_Server.md)
- [Queries](03_Queries.md)

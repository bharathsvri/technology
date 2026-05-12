# MERGE Statement (SQL Server)

`MERGE` performs **insert**, **update**, or **delete** actions on a target table based on **join results** against a source query—useful for **upserts** and **synchronizations**.

## Basic Shape

```sql
MERGE dbo.Customers AS tgt
USING (VALUES (@Id, @Name)) AS src(Id, Name)
  ON tgt.Id = src.Id
WHEN MATCHED THEN
  UPDATE SET Name = src.Name
WHEN NOT MATCHED BY TARGET THEN
  INSERT (Id, Name) VALUES (src.Id, src.Name)
WHEN NOT MATCHED BY SOURCE AND tgt.Archived = 0 THEN
  DELETE;
```

> Real production `MERGE` is often simpler (no `DELETE` branch) to avoid accidental data loss.

## Common Pitfalls
- **Race conditions**: concurrent `MERGE` without proper locking/isolation can still duplicate rows—unique indexes + careful transaction scope matter.
- **Triggers fire** per operation type (`INSERT`/`UPDATE`/`DELETE`)—test trigger logic.
- **Performance**: complex `MERGE` plans can be worse than explicit `UPDATE` + `INSERT` patterns—benchmark.

## Alternatives
- Single-row: `IF EXISTS` + `UPDATE` / `INSERT`.
- Bulk: `INSERT ... SELECT` + separate cleanup, or staging tables + set-based operations.

## Related Topics
- `03_Queries.md`
- `09_Transactions.md`
- `12_Dynamic_SQL.md`

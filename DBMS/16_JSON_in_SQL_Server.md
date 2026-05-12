# JSON in SQL Server (`FOR JSON` / `OPENJSON`)

## Why JSON in the Database?
- **API integration**: shape relational rows as JSON payloads.
- **Semi-structured attributes**: store flexible attributes with optional relational indexing via computed columns + indexes.

## Relational → JSON (`FOR JSON`)
```sql
SELECT c.CustomerId, c.Name, o.OrderId
FROM dbo.Customers c
JOIN dbo.Orders o ON o.CustomerId = c.CustomerId
WHERE c.CustomerId = 123
FOR JSON PATH, ROOT('payload');
```

- **`PATH`**: nested objects via column aliases with dots (`'Order.OrderId'`).
- **`AUTO`**: heuristic nesting based on join order.
- **`WITHOUT_ARRAY_WRAPPER`**: single object instead of array when appropriate.

## JSON → Relational (`OPENJSON`)
```sql
DECLARE @json nvarchar(max) = N'[
  {"sku":"A1","qty":2},
  {"sku":"B2","qty":1}
]';

SELECT j.sku, j.qty
FROM OPENJSON(@json)
WITH (
  sku varchar(20) '$.sku',
  qty int '$.qty'
) AS j;
```

Use **`OPENJSON` with explicit schema** (`WITH`) for predictable typing.

## Storing JSON in Columns
- **`nvarchar(max)`** holding JSON text.
- Validate in app tier or constrain with **`ISJSON`** check constraint.

```sql
ALTER TABLE dbo.EventLog
ADD CONSTRAINT CK_EventLog_PayloadJson
CHECK (PayloadJson IS NULL OR ISJSON(PayloadJson) = 1);
```

## Indexing JSON Properties
Use **computed columns** that extract `JSON_VALUE` / `JSON_QUERY`, then index those columns for hot filters.

## Performance Notes
- Large JSON documents in-row can pressure **buffer pool**; consider size limits.
- Prefer relational normalization for **highly relational** data with many joins and constraints.

## Related Topics
- `03_Queries.md`
- `07_Indexes.md`

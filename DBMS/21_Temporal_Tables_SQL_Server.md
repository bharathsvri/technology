# Temporal Tables (System-Versioned) in SQL Server

**System-versioned temporal tables** keep a **current** row in the base table and maintain **historical versions** automatically in a **history table** with **valid-from / valid-to** system timestamps.

## Why Use Them?
- **Audit** “what did this row look like at time T?” without manual triggers on every column.
- **Slowly changing dimensions** patterns for analytics.
- Regulatory or operational **time travel** queries.

## Creation Sketch

```sql
CREATE TABLE dbo.Customer (
    CustomerId int NOT NULL PRIMARY KEY,
    Name nvarchar(200) NOT NULL,
    SysStart datetime2 GENERATED ALWAYS AS ROW START NOT NULL,
    SysEnd   datetime2 GENERATED ALWAYS AS ROW END NOT NULL,
    PERIOD FOR SYSTEM_TIME (SysStart, SysEnd)
)
WITH (SYSTEM_VERSIONING = ON (HISTORY_TABLE = dbo.CustomerHistory));
```

## Querying As Of

```sql
SELECT *
FROM dbo.Customer
FOR SYSTEM_TIME AS OF '2026-05-01T12:00:00';
```

## Operational Notes
- History table **grows** forever—plan **retention** (partitioning, stretch, periodic purge with policy).
- Index both **current** and **history** tables appropriately for your access paths.

## Related Topics
- `06_Triggers.md` (alternative audit approach)
- `15_Table_Partitioning.md` (retention strategies)

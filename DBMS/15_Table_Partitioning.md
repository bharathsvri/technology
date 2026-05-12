# Table Partitioning (SQL Server Overview)

## Why Partition?
**Partitioning** splits one logical table into multiple **physical rowsets** while keeping a single table name for queries. Goals:
- **Manageability**: switch partitions in/out for archival (`SWITCH`).
- **Performance**: partition elimination when queries filter on the partition key.
- **Maintenance**: rebuild indexes per partition; update statistics strategically.

## Partition Function + Scheme
1. **Partition function**: maps column values to **boundary points** (`RANGE LEFT` / `RANGE RIGHT`).
2. **Partition scheme**: maps function ranges to **filegroups**.

```sql
CREATE PARTITION FUNCTION PF_OrderDate (datetime2)
AS RANGE RIGHT FOR VALUES ('2024-01-01','2025-01-01','2026-01-01');

CREATE PARTITION SCHEME PS_OrderDate
AS PARTITION PF_OrderDate
ALL TO ([PRIMARY]);

CREATE TABLE dbo.Orders (
    OrderId bigint NOT NULL,
    OrderDate datetime2 NOT NULL,
    ...
) ON PS_OrderDate(OrderDate);
```

## Clustered Index Alignment
Partitioning column is usually the **clustered index leading key** (here `OrderDate`) so inserts align with partition boundaries and elimination works predictably.

## Sliding Window Pattern
For time-series data:
- **SPLIT RANGE** to add a new boundary.
- **MERGE RANGE** to retire old boundaries after archive.
- Use **`SWITCH`** to move whole partitions between staging and archive tables quickly (metadata-only when aligned).

## Query Performance Caveats
- Partitioning **does not** replace missing indexes on selective filters.
- Poor partition keys (high cardinality random values) cause **many partitions touched** → worse than non-partitioned.

## Columnstore (Brief)
**Clustered columnstore** suits large fact tables (analytics). Pair with partitioning for partition switch loads and segment elimination.

## Related Topics
- `07_Indexes.md`
- `13_Temporary_Tables.md` (staging patterns)

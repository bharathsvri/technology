# Columnstore Indexes (SQL Server Overview)

**Columnstore** stores and compresses data **per column** (column segments) rather than only row-wise pages. It excels at **scan-heavy analytics** and **star-schema** fact tables.

## Rowstore vs Columnstore (Rule of Thumb)
- **Rowstore (B-tree)**: OLTP point lookups, small selective reads/writes.
- **Columnstore**: large **aggregations**, scans, reporting workloads.

## Types
- **Nonclustered columnstore** on a rowstore table (hybrid scenarios).
- **Clustered columnstore** (often fact tables with **bulk insert** patterns).

## Batch Mode Execution
Queries using columnstore can leverage **batch mode** operators (vectorized processing). Modern cardinality estimation and **compatibility level** affect batch-mode eligibility—validate on your version.

## Maintenance
- **Rebuild/reorganize** columnstore differently than rowstore—monitor **deleted bitmap** / **tombstone** rowgroups and **segment** quality.
- Partitioning pairs well with **partition switch** loads.

## When Not to Use
- Highly volatile narrow tables with constant single-row updates—**deltas** to deltastore and compression overhead can hurt.

## Related Topics
- `07_Indexes.md`
- `15_Table_Partitioning.md`
- `17_Execution_Plans_and_Query_Store.md`

# Execution Plans, Statistics, and Query Store (SQL Server)

## Execution Plans
The optimizer produces a plan: join order, index seeks/scans, sorts, parallelism.

### Viewing Plans
- **Estimated plan**: quick, uses current statistics (no execution).
- **Actual plan**: includes **runtime** row counts, spills, actual parallelism—use for tuning real skew.

Common entry points: SSMS “Include Actual Execution Plan”, `SET STATISTICS IO, TIME ON` for I/O/time messaging.

## Reading Plans (Pragmatic Checklist)
- **Warnings**: cardinality mis-estimates, implicit conversions, spills to tempdb.
- **Fat arrows**: actual rows ≫ estimated rows → **statistics** or **parameter sniffing** issues.
- **Key lookups**: consider covering indexes when selective enough.
- **Scan vs seek**: scans acceptable on small tables or wide analytical queries; problematic on hot OLTP paths.

## Statistics
**Cardinality estimation** depends on histograms and density vectors.

### Maintenance
- **`UPDATE STATISTICS`** for large shifts after bulk loads.
- Avoid blind `FULLSCAN` everywhere; use targeted updates with **sample** where appropriate.
- Modern workloads: consider **auto-update** thresholds but validate ETL windows.

## Parameter Sniffing (Concept)
First compiled plan may favor one parameter value; later values perform poorly. Mitigations include:
- **`OPTION (RECOMPILE)`** for volatile TVPs/small procedures (trade CPU).
- **`OPTIMIZE FOR UNKNOWN`** / **`OPTIMIZE FOR (@p = literal)`** (scenario-specific).
- **Plan guides** (last resort).

## Query Store (Highly Recommended)
Captures **plan history**, compile/runtime stats, and supports:
- **Regressions detection**: compare plans over time.
- **Forced plans**: pin a known-good plan (use cautiously).
- **Atlas of wait categories** per query.

Enable at database level:

```sql
ALTER DATABASE [YourDb] SET QUERY_STORE = ON;
```

## Extended Events (Brief)
Lightweight tracing for statement completion, waits, and attention events—complements Query Store.

## Related Topics
- `07_Indexes.md`
- `09_Transactions.md`
- `03_Queries.md`

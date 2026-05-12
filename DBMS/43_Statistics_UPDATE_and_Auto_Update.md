# Statistics: `UPDATE STATISTICS` and Auto-Update

The **optimizer** uses **histograms** and **density** information to estimate cardinalities—**stale stats** cause **bad plans** (nested loops vs hash joins wrong).

---

## Manual maintenance

```sql
UPDATE STATISTICS dbo.Orders WITH FULLSCAN; -- heavy; use in controlled windows
```

**`SAMPLE`** options reduce scan cost at accuracy trade-off.

---

## Auto update thresholds

SQL Server **auto-updates** stats when enough row changes occur—**very large** tables may lag—some teams use **jobs** + **`sp_updatestats`** strategically.

---

## Related

- [Execution Plans and Query Store](17_Execution_Plans_and_Query_Store.md)
- [Parameter Sniffing and OPTION RECOMPILE](34_Parameter_Sniffing_and_OPTION_RECOMPILE.md)

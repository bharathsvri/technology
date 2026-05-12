# Backup and Restore (SQL Server)

Backups are the foundation of **RPO/RTO** planning: how much data you can afford to lose (**Recovery Point Objective**) and how fast you must be online (**Recovery Time Objective**).

## Backup Types
1. **Full database backup**: baseline of the whole database.
2. **Differential backup**: changes since the **last full** backup (faster, smaller than repeated fulls when used in a chain).
3. **Transaction log backup** (FULL recovery model): captures log since prior log backup—enables **point-in-time restore**.

### COPY_ONLY
Use **`COPY_ONLY`** for ad-hoc full backups that **do not** break the differential base chain (common for one-off copies to staging).

## Recovery Models (Brief)
- **SIMPLE**: log reuse is automatic; **no** point-in-time restores via log chain.
- **FULL / BULK_LOGGED**: log backups supported; typical for production OLTP with RPO < 24h.

## Example: Full + Log Strategy (Conceptual)

```sql
BACKUP DATABASE [AppDb]
  TO DISK = N'\\backup\share\AppDb_Full.bak'
  WITH INIT, CHECKSUM, COMPRESSION;

BACKUP LOG [AppDb]
  TO DISK = N'\\backup\share\AppDb_Log.trn'
  WITH INIT, CHECKSUM, COMPRESSION;
```

## Restore Outline
1. **RESTORE DATABASE ... WITH NORECOVERY** — apply full (and diffs).
2. **RESTORE LOG ... WITH NORECOVERY** — replay logs in order.
3. Final **`WITH RECOVERY`** — bring database online (or `STANDBY` for reporting).

### Point-in-Time

```sql
RESTORE LOG [AppDb] FROM DISK = N'\\backup\share\AppDb_Log_010.trn'
  WITH STOPAT = '2026-05-12T14:30:00', RECOVERY;
```

## Verification
- **`RESTORE VERIFYONLY`** validates backup integrity without restoring data files.
- Prefer **`CHECKSUM`** during backups to detect page corruption early.

## High Availability Note
**Always On Availability Groups** shift some operational patterns (shared storage vs direct attached), but **backups** remain mandatory for logical corruption and human error.

## Related Topics
- `09_Transactions.md` (transaction log role during recovery)
- `14_Security.md` (backup operator roles)

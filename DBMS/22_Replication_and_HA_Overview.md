# Replication and High Availability Overview (SQL Server)

This is a **concept map** of how SQL Server keeps data **available** and **copied** across nodes. Product names and features vary by edition—validate licensing and edition limits.

## Log Shipping
- Ships **transaction log backups** to a standby server and restores them.
- Simple DR pattern; **RPO** depends on log backup frequency.

## Database Mirroring (Legacy)
- Principal/mirror partnership with automatic failover in high-safety modes.
- Mostly superseded by **Always On** for new designs, but you may still see it in older estates.

## Always On Availability Groups (AG)
- Groups of databases fail over together to **replicas**.
- **Synchronous** commit for zero data loss where network latency allows; **asynchronous** for DR distance.
- **Readable secondaries** offload reporting (with isolation/correctness considerations).

## Failover Cluster Instances (FCI)
- **Shared storage** cluster with one active SQL instance at a time—protects **instance** hardware, not storage corruption the same way AG can.

## Replication (Transactional / Merge / Snapshot)
- **Transactional replication**: near real-time distribution for reporting or edge databases.
- **Merge replication**: offline/subscriber scenarios (complex conflict resolution).
- **Snapshot**: baseline copies; often combined with transactional.

## Choosing a Strategy (Rule of Thumb)
- **HA inside a data center**: FCI or AG depending on storage model and ops maturity.
- **DR across sites**: AG async, log shipping, or storage replication—balance RPO/RTO vs cost.

## Related Topics
- `18_Backup_and_Restore_SQL_Server.md`
- `19_Locking_Blocking_Deadlocks.md`
- `14_Security.md` (endpoint and replica security)

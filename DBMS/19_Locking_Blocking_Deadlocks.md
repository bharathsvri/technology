# Locking, Blocking, and Deadlocks (SQL Server)

## Why Locks Exist
The engine uses **locks** to implement **isolation** while preserving **ACID** semantics. Reads and writes acquire locks on rows, pages, keys, or tables depending on plan and hints.

## Common Lock Modes (Conceptual)
- **Shared (S)**: compatible with other readers; blocks writers.
- **Exclusive (X)**: writers; blocks conflicting readers/writers.
- **Update (U)**: often used as an upgrade path to reduce certain deadlock patterns during seeks.

## Blocking vs Deadlock
- **Blocking**: session A holds a lock session B needs; B waits. Often resolves when A commits/rolls back—**tune duration** of transactions and indexes.
- **Deadlock**: cyclic wait (A waits on B, B waits on A). SQL Server **detects** and kills one victim (`1205` error).

## Isolation Levels (Reminder)
Rough behavior (see `09_Transactions.md` for ACID context):
- **READ COMMITTED** (default in many installs): no dirty reads; writers block readers depending on row versioning settings.
- **REPEATABLE READ / SERIALIZABLE**: stronger guarantees; more locking risk.

**Read committed snapshot (RCSI)** using **row versioning** reduces **read/write blocking** at the cost of **tempdb** version store maintenance.

## Diagnosing Hotspots
- **`sys.dm_tran_locks`**, **`sys.dm_os_waiting_tasks`**: who waits on what.
- **Extended Events** (`blocked_process_report`, `xml_deadlock_report`): capture blocking chains and deadlock graphs.
- **Activity Monitor** / third-party tools for live views.

## Practical Mitigations
- Keep transactions **short**; avoid chatty patterns inside transactions.
- Ensure **indexes** support selective seeks (reduce scans that escalate locks).
- Access tables in a **consistent order** across procedures to lower deadlock probability.
- Retry **deadlock victims** idempotently in application code when safe.

## Related Topics
- `09_Transactions.md`
- `07_Indexes.md`
- `17_Execution_Plans_and_Query_Store.md`

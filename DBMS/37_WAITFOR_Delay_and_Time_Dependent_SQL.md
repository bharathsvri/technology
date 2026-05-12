# `WAITFOR` and Testing Time-Dependent SQL

**`WAITFOR DELAY`** pauses batch execution—useful for **reproducing** race conditions in **test** environments, dangerous in **production** code paths.

---

## Syntax

```sql
WAITFOR DELAY '00:00:02'; -- 2 seconds
```

**`WAITFOR TIME`** runs block after a clock time—rare in apps.

---

## Alternatives for throttling

Prefer **application-level** backoff, **Service Broker** activation timing, or **Agent schedules**—not sleeping sessions holding **locks**.

---

## Testing tip

Combine with **two sessions** scripts to **force** deadlock or **blocking** demos—document **SET LOCK_TIMEOUT** to avoid hung labs.

---

## Related

- [Transactions](09_Transactions.md)
- [Locking Blocking Deadlocks](19_Locking_Blocking_Deadlocks.md)

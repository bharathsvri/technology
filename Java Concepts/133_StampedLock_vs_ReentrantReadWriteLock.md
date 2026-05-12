# `StampedLock` vs `ReentrantReadWriteLock`

**`StampedLock`** (Java 8+) offers **optimistic reading**—a middle ground between **`volatile`** reads and full **write locks**—with strict **validation** rules.

---

## Modes

- **Writing**: `writeLock()` exclusive.
- **Reading**: `readLock()` shared (similar to RRWL).
- **Optimistic read**: `tryOptimisticRead()` + **`validate(stamp)`** after read—if invalid, **upgrade** to proper read lock.

---

## When optimistic helps

**Read-mostly** data where **writes are rare**—avoid **full** read lock cost when no concurrent write occurred.

---

## Pitfalls

- **API is easy to misuse**—wrong stamp handling → **exceptions** / deadlock risk.
- **Not reentrant**—unlike **`ReentrantReadWriteLock`**.
- Prefer **higher-level** structures when team familiarity is low.

---

## Related

- [Multithreading](21_Multithreading.md)
- [Advanced Multithreading](35_Advanced_Multithreading.md)

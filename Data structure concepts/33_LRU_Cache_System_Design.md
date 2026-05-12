# LRU Cache — Design and Implementation

**LRU (Least Recently Used)** evicts the item that has not been used for the longest time when capacity is exceeded. Classic interview: implement **O(1)** `get` and `put`.

---

## Requirements (restated)

- `get(key)` → value or miss indicator.
- `put(key, value)` → insert or update; if size **> capacity**, evict **LRU** entry.
- All operations **O(1)** average.

---

## Data structures

1. **Hash map**: `key → list node` (or key → value + node pointer).
2. **Doubly linked list**: nodes ordered **MRU (head)** → **LRU (tail)**.

**Why doubly linked?** Remove a node in **O(1)** when promoting on `get` or updating on `put`.

### Operations sketch

- **get hit**: move node to MRU side (detach + insert front).
- **put new when full**: evict **tail** (LRU), remove its key from map, then insert new at head.
- **put existing**: update value + move to MRU.

---

## Complexity

- Time: **O(1)** average per op (hash + pointer updates).
- Space: **O(capacity)**.

---

## Distributed / system design angle

- **Single JVM**: this structure is enough.
- **Distributed cache** (Redis, Memcached): LRU is **approximate** per node; use **TTL**, **consistent hashing**, **singleflight** for hot keys.
- **Stampede**: many clients miss cache simultaneously — **probabilistic early expiration**, locks, or **request coalescing**.

---

## JDK note

`LinkedHashMap` with `accessOrder=true` and `removeEldestEntry` implements LRU for prototypes; interviews usually want **explicit** list + map.

---

## Related

- `12_Maps_Dictionaries.md`, `02_Linked_Lists.md`, `08_Hash_Tables.md`

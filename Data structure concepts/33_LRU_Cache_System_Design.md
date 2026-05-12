# LRU Cache (Concept + System Design)

## Requirements (Typical Interview Spec)
Implement **Least Recently Used (LRU)** cache with:
- `get(key)` → value or miss
- `put(key, value)` → evicts LRU item if at capacity
All operations **O(1)** average.

## Data Structures
Combine:
1. **Hash map** from `key` → **list node** (or key → value + linked position).
2. **Doubly linked list** of keys ordered from **MRU (head)** to **LRU (tail)**.

### Why Doubly Linked?
You must **detach** a node in **O(1)** when promoting it to most-recently-used after a `get` or `put`.

## Operations
- **get hit**: move node to MRU side.
- **get miss**: return sentinel (e.g., `-1`).
- **put**:
  - If key exists, update value + promote to MRU.
  - Else insert new node at MRU; if size > capacity, **remove LRU tail** and delete its key from map.

## JDK Alternative (Know the Semantics)
`LinkedHashMap` with `accessOrder=true` and `removeEldestEntry` can implement LRU quickly for prototypes, but interviews usually want the **explicit** list + map model.

## System Design Angles
- **Distributed LRU**: not exact without shared state; use **Redis** `volatile-lru` / `allkeys-lru` or client-side caches with TTLs.
- **Stampede prevention**: singleflight / locks around rebuild of hot keys.
- **Eviction policy trade-offs**: LRU vs LFU vs **TinyLFU** (Caffeine) in JVM apps.

## Complexity
- Time: **O(1)** average per op (hash map + pointer updates).
- Space: **O(capacity)**.

## Related Topics
- `12_Maps_Dictionaries.md`
- `02_Linked_Lists.md`
- [Spring Boot — Redis](../Spring%20Boot%20Concepts/29-Spring-Boot-Redis.md) for operational caching patterns

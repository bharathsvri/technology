# `LinkedHashMap` and LRU Ordering

**`LinkedHashMap`** maintains **insertion or access order** via a **doubly linked list** threading entries—foundation for **LRU caches** and predictable **iteration**.

---

## Access-order mode

```java
new LinkedHashMap<>(16, 0.75f, true); // accessOrder = true
```

**`get`** moves entry to tail—**LRU** at **head** when combined with **`removeEldestEntry`** override.

---

## JDK `LinkedHashMap` as LRU (pattern)

Override **`removeEldestEntry`** to cap size—**not** thread-safe; use **`Collections.synchronizedMap`** or **`ConcurrentHashMap`** + external policy for concurrency.

---

## Interview link

Connect to **`33_LRU_Cache_System_Design.md`** in DSA notes—production caches use **Caffeine** / **Redis**, but **`LinkedHashMap`** is the teaching model.

---

## Related

- [Collections Framework](17_Collections_Framework.md)
- [LRU Cache (DSA)](../Data%20structure%20concepts/33_LRU_Cache_System_Design.md)

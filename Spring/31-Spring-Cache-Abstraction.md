# Spring Cache Abstraction (`@Cacheable`)

Spring’s **cache abstraction** decouples **annotation-driven** caching from the backing store (**ConcurrentHashMap**, **Caffeine**, **Redis**, etc.) via **`CacheManager`** beans.

---

## Core annotations

- **`@Cacheable`**: on cache hit, method body **skipped**; key from params (default) or **`key`** SpEL.
- **`@CachePut`**: always executes method, **updates** cache.
- **`@CacheEvict`**: removes entries (single or **`allEntries=true`**).

---

## Configuration sketch

```java
@Configuration
@EnableCaching
public class CacheConfig {
  @Bean
  public CacheManager cacheManager() {
    return new ConcurrentMapCacheManager("users", "reports");
  }
}
```

Production often uses **Caffeine** or **Redis** with **TTL** and **size** policies.

---

## Pitfalls

- **Self-invocation** (`this.foo()` inside same class) **bypasses proxy** → annotations ignored; refactor or **`AopContext`**.

- **`equals`/`hashCode`** on **cache keys** (custom types) must be stable.

- **Stale data**: pair **`@CacheEvict`** with write paths or use **short TTL**.

---

## Related

- [Spring Boot Caching](../Spring%20Boot%20Concepts/11-Spring-Boot-Caching.md)
- [Aspect-Oriented Programming](08-Aspect-Oriented-Programming.md)

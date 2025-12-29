# Google Guava

## What is Guava?
Guava is a set of core libraries from Google that includes collections, caching, primitives support, concurrency, common annotations, string processing, and I/O.

## Collections
```java
import com.google.common.collect.*;

// Immutable collections
ImmutableList<String> list = ImmutableList.of("a", "b", "c");
ImmutableSet<String> set = ImmutableSet.of("a", "b", "c");
ImmutableMap<String, Integer> map = ImmutableMap.of("a", 1, "b", 2);

// Multimap
Multimap<String, String> multimap = ArrayListMultimap.create();
multimap.put("key", "value1");
multimap.put("key", "value2");

// BiMap
BiMap<String, Integer> biMap = HashBiMap.create();
biMap.put("one", 1);
Integer value = biMap.get("one");
String key = biMap.inverse().get(1);
```

## Caching
```java
import com.google.common.cache.*;

Cache<String, String> cache = CacheBuilder.newBuilder()
    .maximumSize(1000)
    .expireAfterWrite(10, TimeUnit.MINUTES)
    .build();

cache.put("key", "value");
String value = cache.getIfPresent("key");

// Loading cache
LoadingCache<String, String> loadingCache = CacheBuilder.newBuilder()
    .maximumSize(1000)
    .build(new CacheLoader<String, String>() {
        @Override
        public String load(String key) {
            return fetchValue(key);
        }
    });

String value = loadingCache.get("key");
```

## String Utilities
```java
import com.google.common.base.*;

// Joiner
String joined = Joiner.on(", ").skipNulls().join("a", null, "b", "c");

// Splitter
Iterable<String> split = Splitter.on(",").trimResults().split("a, b, c");

// Strings
Strings.isNullOrEmpty(str);
Strings.padStart(str, 10, '0');
Strings.repeat("a", 3);
```

## Preconditions
```java
import com.google.common.base.Preconditions;

public void method(String name, int age) {
    Preconditions.checkNotNull(name, "Name cannot be null");
    Preconditions.checkArgument(age > 0, "Age must be positive: %s", age);
    Preconditions.checkState(isValid(), "State is invalid");
}
```

## Best Practices
1. Use immutable collections when possible
2. Use caching for expensive operations
3. Use preconditions for validation
4. Leverage Guava utilities
5. Understand performance characteristics
6. Use appropriate collection types
7. Handle nulls properly
8. Read Guava documentation


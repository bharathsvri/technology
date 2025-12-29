# Spring Boot Caching

## What is Caching?

Caching is a mechanism to store frequently accessed data in memory for faster retrieval. It reduces database load and improves application performance.

## Enabling Caching in Spring Boot

### Add Dependency

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-cache</artifactId>
</dependency>
```

### Enable Caching

```java
@SpringBootApplication
@EnableCaching
public class Application {
    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}
```

## Cache Providers

### Simple Cache (Default)

No additional configuration needed. Uses ConcurrentHashMap.

### Redis Cache

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
```

```properties
spring.cache.type=redis
spring.redis.host=localhost
spring.redis.port=6379
```

### Caffeine Cache

```xml
<dependency>
    <groupId>com.github.ben-manes.caffeine</groupId>
    <artifactId>caffeine</artifactId>
</dependency>
```

```properties
spring.cache.type=caffeine
spring.cache.caffeine.spec=maximumSize=500,expireAfterWrite=5m
```

### EhCache

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-cache</artifactId>
</dependency>
<dependency>
    <groupId>net.sf.ehcache</groupId>
    <artifactId>ehcache</artifactId>
</dependency>
```

## Cache Annotations

### @Cacheable

Caches method result:

```java
@Cacheable(value = "users", key = "#id")
public User getUserById(Long id) {
    return userRepository.findById(id).orElse(null);
}
```

### @CacheEvict

Removes entries from cache:

```java
@CacheEvict(value = "users", key = "#id")
public void deleteUser(Long id) {
    userRepository.deleteById(id);
}

@CacheEvict(value = "users", allEntries = true)
public void deleteAllUsers() {
    userRepository.deleteAll();
}
```

### @CachePut

Updates cache with method result:

```java
@CachePut(value = "users", key = "#user.id")
public User updateUser(User user) {
    return userRepository.save(user);
}
```

### @Caching

Combines multiple cache operations:

```java
@Caching(
    evict = {
        @CacheEvict(value = "users", key = "#id"),
        @CacheEvict(value = "userList", allEntries = true)
    }
)
public void deleteUser(Long id) {
    userRepository.deleteById(id);
}
```

### @CacheConfig

Configures cache settings at class level:

```java
@Service
@CacheConfig(cacheNames = "users")
public class UserService {
    
    @Cacheable
    public User getUserById(Long id) {
        // Uses "users" cache
    }
}
```

## Cache Key Generation

### Default Key Generation

Uses all method parameters:

```java
@Cacheable("users")
public User getUser(String name, String email) {
    // Key: name + email
}
```

### Custom Key

```java
@Cacheable(value = "users", key = "#id")
public User getUserById(Long id) {
    // Key: id
}

@Cacheable(value = "users", key = "#user.id")
public User getUser(User user) {
    // Key: user.id
}

@Cacheable(value = "users", key = "#p0")
public User getUserById(Long id) {
    // Key: first parameter
}
```

### Complex Key

```java
@Cacheable(value = "users", key = "#id + '_' + #type")
public User getUserByIdAndType(Long id, String type) {
    // Key: id_type
}
```

## Conditional Caching

### @Cacheable with Condition

```java
@Cacheable(value = "users", condition = "#id > 10")
public User getUserById(Long id) {
    // Only caches if id > 10
}

@Cacheable(value = "users", unless = "#result == null")
public User getUserById(Long id) {
    // Caches unless result is null
}
```

### @CacheEvict with Condition

```java
@CacheEvict(value = "users", condition = "#id > 10")
public void deleteUser(Long id) {
    // Only evicts if id > 10
}
```

## Cache Configuration

### Simple Cache Configuration

```java
@Configuration
@EnableCaching
public class CacheConfig {
    
    @Bean
    public CacheManager cacheManager() {
        SimpleCacheManager cacheManager = new SimpleCacheManager();
        cacheManager.setCaches(Arrays.asList(
            new ConcurrentMapCache("users"),
            new ConcurrentMapCache("products")
        ));
        return cacheManager;
    }
}
```

### Redis Cache Configuration

```java
@Configuration
@EnableCaching
public class RedisCacheConfig {
    
    @Bean
    public RedisConnectionFactory redisConnectionFactory() {
        return new LettuceConnectionFactory();
    }
    
    @Bean
    public CacheManager cacheManager(RedisConnectionFactory connectionFactory) {
        RedisCacheConfiguration config = RedisCacheConfiguration.defaultCacheConfig()
            .entryTtl(Duration.ofMinutes(10))
            .serializeKeysWith(RedisSerializationContext.SerializationPair
                .fromSerializer(new StringRedisSerializer()))
            .serializeValuesWith(RedisSerializationContext.SerializationPair
                .fromSerializer(new GenericJackson2JsonRedisSerializer()));
        
        return RedisCacheManager.builder(connectionFactory)
            .cacheDefaults(config)
            .build();
    }
}
```

### Caffeine Cache Configuration

```java
@Configuration
@EnableCaching
public class CaffeineCacheConfig {
    
    @Bean
    public CacheManager cacheManager() {
        CaffeineCache usersCache = new CaffeineCache("users",
            Caffeine.newBuilder()
                .maximumSize(1000)
                .expireAfterWrite(10, TimeUnit.MINUTES)
                .build());
        
        CaffeineCache productsCache = new CaffeineCache("products",
            Caffeine.newBuilder()
                .maximumSize(500)
                .expireAfterWrite(5, TimeUnit.MINUTES)
                .build());
        
        SimpleCacheManager cacheManager = new SimpleCacheManager();
        cacheManager.setCaches(Arrays.asList(usersCache, productsCache));
        return cacheManager;
    }
}
```

## Complete Example

### Service with Caching

```java
@Service
@CacheConfig(cacheNames = "users")
public class UserService {
    
    private final UserRepository userRepository;
    
    public UserService(UserRepository userRepository) {
        this.userRepository = userRepository;
    }
    
    @Cacheable(key = "#id")
    public User getUserById(Long id) {
        System.out.println("Fetching user from database: " + id);
        return userRepository.findById(id)
            .orElseThrow(() -> new ResourceNotFoundException("User not found"));
    }
    
    @Cacheable(key = "#name")
    public User getUserByName(String name) {
        System.out.println("Fetching user from database: " + name);
        return userRepository.findByName(name)
            .orElseThrow(() -> new ResourceNotFoundException("User not found"));
    }
    
    @CachePut(key = "#user.id")
    public User updateUser(User user) {
        System.out.println("Updating user: " + user.getId());
        return userRepository.save(user);
    }
    
    @CacheEvict(key = "#id")
    public void deleteUser(Long id) {
        System.out.println("Deleting user: " + id);
        userRepository.deleteById(id);
    }
    
    @CacheEvict(allEntries = true)
    public void clearCache() {
        System.out.println("Clearing all cache");
    }
}
```

## Cache Statistics

### Using Cache Statistics

```java
@Autowired
private CacheManager cacheManager;

public void printCacheStatistics() {
    Cache cache = cacheManager.getCache("users");
    if (cache instanceof CaffeineCache) {
        CaffeineCache caffeineCache = (CaffeineCache) cache;
        com.github.benmanes.caffeine.cache.stats.CacheStats stats = 
            caffeineCache.getNativeCache().stats();
        System.out.println("Hit count: " + stats.hitCount());
        System.out.println("Miss count: " + stats.missCount());
    }
}
```

## Cache Warming

### Pre-loading Cache

```java
@Component
public class CacheWarmup {
    
    @Autowired
    private UserService userService;
    
    @PostConstruct
    public void warmupCache() {
        // Pre-load frequently accessed data
        List<Long> popularUserIds = Arrays.asList(1L, 2L, 3L);
        popularUserIds.forEach(userService::getUserById);
    }
}
```

## Best Practices

1. **Choose Right Cache Provider**: Select based on requirements
2. **Set Appropriate TTL**: Set time-to-live based on data freshness needs
3. **Use Conditional Caching**: Cache only when appropriate
4. **Evict Stale Data**: Use `@CacheEvict` when data changes
5. **Monitor Cache Performance**: Track hit/miss ratios
6. **Use Cache Keys Wisely**: Design keys for efficient lookups
7. **Handle Cache Failures**: Implement fallback mechanisms
8. **Test Cache Behavior**: Verify caching works as expected

## Cache Abstraction

### Custom Cache Resolver

```java
@Component
public class CustomCacheResolver implements CacheResolver {
    
    @Override
    public Collection<? extends Cache> resolveCaches(CacheOperationInvocationContext<?> context) {
        // Custom cache resolution logic
        return Arrays.asList(cacheManager.getCache("users"));
    }
}
```

## Summary

Spring Boot Caching:
- Reduces database load
- Improves application performance
- Supports multiple cache providers
- Provides declarative caching annotations
- Easy to configure and use
- Essential for high-performance applications


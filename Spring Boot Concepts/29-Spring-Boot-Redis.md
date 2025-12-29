# Spring Boot Redis

## What is Redis?

Redis is an in-memory data structure store that can be used as a database, cache, and message broker. It's extremely fast and supports various data structures.

## Setting Up Redis

### Add Dependency

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
```

### Configuration

```properties
# Redis Configuration
spring.data.redis.host=localhost
spring.data.redis.port=6379
spring.data.redis.password=
spring.data.redis.database=0
spring.data.redis.timeout=2000ms
```

## Redis Template

### Basic Configuration

```java
@Configuration
public class RedisConfig {
    
    @Bean
    public RedisTemplate<String, Object> redisTemplate(RedisConnectionFactory connectionFactory) {
        RedisTemplate<String, Object> template = new RedisTemplate<>();
        template.setConnectionFactory(connectionFactory);
        template.setDefaultSerializer(new GenericJackson2JsonRedisSerializer());
        template.setKeySerializer(new StringRedisSerializer());
        template.setHashKeySerializer(new StringRedisSerializer());
        template.setValueSerializer(new GenericJackson2JsonRedisSerializer());
        return template;
    }
}
```

### Using RedisTemplate

```java
@Service
public class RedisService {
    
    private final RedisTemplate<String, Object> redisTemplate;
    
    public RedisService(RedisTemplate<String, Object> redisTemplate) {
        this.redisTemplate = redisTemplate;
    }
    
    public void setValue(String key, Object value) {
        redisTemplate.opsForValue().set(key, value);
    }
    
    public Object getValue(String key) {
        return redisTemplate.opsForValue().get(key);
    }
    
    public void deleteValue(String key) {
        redisTemplate.delete(key);
    }
    
    public void setValueWithExpiry(String key, Object value, long timeout, TimeUnit unit) {
        redisTemplate.opsForValue().set(key, value, timeout, unit);
    }
}
```

## Redis Repository

### Entity

```java
@RedisHash("user")
public class User {
    @Id
    private String id;
    private String name;
    private String email;
    // Constructors, getters, setters
}
```

### Repository

```java
public interface UserRepository extends CrudRepository<User, String> {
    List<User> findByName(String name);
    List<User> findByEmail(String email);
}
```

## Caching with Redis

### Configuration

```java
@Configuration
@EnableCaching
public class RedisCacheConfig {
    
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

### Using Cache

```java
@Service
public class UserService {
    
    @Cacheable(value = "users", key = "#id")
    public User getUserById(String id) {
        return userRepository.findById(id).orElse(null);
    }
    
    @CacheEvict(value = "users", key = "#id")
    public void deleteUser(String id) {
        userRepository.deleteById(id);
    }
}
```

## Session Management

### Spring Session with Redis

#### Add Dependency

```xml
<dependency>
    <groupId>org.springframework.session</groupId>
    <artifactId>spring-session-data-redis</artifactId>
</dependency>
```

#### Configuration

```java
@Configuration
@EnableRedisHttpSession
public class SessionConfig {
}
```

## Message Queue

### Redis Pub/Sub

```java
@Service
public class RedisMessageService {
    
    private final RedisTemplate<String, Object> redisTemplate;
    
    public void publish(String channel, Object message) {
        redisTemplate.convertAndSend(channel, message);
    }
}

@Component
public class RedisMessageListener implements MessageListener {
    
    @Override
    public void onMessage(Message message, byte[] pattern) {
        System.out.println("Received message: " + new String(message.getBody()));
    }
}
```

## Best Practices

1. **Connection Pooling**: Configure connection pooling
2. **Serialization**: Use efficient serialization
3. **Key Naming**: Use consistent key naming conventions
4. **TTL**: Set appropriate TTL for cached data
5. **Monitor Performance**: Monitor Redis performance

## Summary

Spring Boot Redis:
- Fast in-memory data store
- Supports caching and session management
- Can be used as message broker
- Essential for high-performance applications


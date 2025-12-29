# Redis with Java

## What is Redis?
Redis is an in-memory data structure store used as a database, cache, and message broker.

## Jedis Client
```java
import redis.clients.jedis.Jedis;

public class RedisExample {
    public static void main(String[] args) {
        Jedis jedis = new Jedis("localhost", 6379);
        
        // String operations
        jedis.set("key", "value");
        String value = jedis.get("key");
        
        // List operations
        jedis.lpush("list", "item1", "item2");
        List<String> list = jedis.lrange("list", 0, -1);
        
        // Set operations
        jedis.sadd("set", "member1", "member2");
        Set<String> set = jedis.smembers("set");
        
        // Hash operations
        jedis.hset("hash", "field", "value");
        String hashValue = jedis.hget("hash", "field");
        
        // Expiration
        jedis.expire("key", 60); // Expire in 60 seconds
        
        jedis.close();
    }
}
```

## Spring Data Redis
```java
import org.springframework.data.redis.core.*;

@Service
public class RedisService {
    
    @Autowired
    private RedisTemplate<String, String> redisTemplate;
    
    public void setValue(String key, String value) {
        redisTemplate.opsForValue().set(key, value);
    }
    
    public String getValue(String key) {
        return redisTemplate.opsForValue().get(key);
    }
    
    public void setValueWithExpiry(String key, String value, long seconds) {
        redisTemplate.opsForValue().set(key, value, seconds, TimeUnit.SECONDS);
    }
    
    public void addToList(String key, String value) {
        redisTemplate.opsForList().rightPush(key, value);
    }
    
    public List<String> getList(String key) {
        return redisTemplate.opsForList().range(key, 0, -1);
    }
}
```

## Caching with Redis
```java
import org.springframework.cache.annotation.*;

@Service
@CacheConfig(cacheNames = "users")
public class UserService {
    
    @Cacheable
    public User findById(Long id) {
        return userRepository.findById(id);
    }
    
    @CacheEvict
    public void delete(Long id) {
        userRepository.deleteById(id);
    }
    
    @CachePut
    public User update(User user) {
        return userRepository.save(user);
    }
}
```

## Best Practices
1. Use connection pooling
2. Set appropriate TTL
3. Use pipelining for bulk operations
4. Monitor memory usage
5. Use Redis Sentinel for HA
6. Implement proper error handling
7. Use appropriate data structures
8. Cache frequently accessed data


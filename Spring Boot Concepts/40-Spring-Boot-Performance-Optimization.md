# Spring Boot Performance Optimization

## Performance Optimization Techniques

Optimizing Spring Boot applications involves various techniques to improve response times, throughput, and resource utilization.

## Database Optimization

### Connection Pooling

```properties
# HikariCP Configuration
spring.datasource.hikari.maximum-pool-size=20
spring.datasource.hikari.minimum-idle=5
spring.datasource.hikari.connection-timeout=30000
spring.datasource.hikari.idle-timeout=600000
spring.datasource.hikari.max-lifetime=1800000
```

### Query Optimization

```java
// Use projections
@Query("SELECT u.name, u.email FROM User u WHERE u.id = :id")
UserProjection findUserProjectionById(@Param("id") Long id);

// Use pagination
Page<User> findAll(Pageable pageable);

// Use batch operations
@Modifying
@Query("UPDATE User u SET u.active = :active WHERE u.id IN :ids")
void updateUsersActiveStatus(@Param("ids") List<Long> ids, @Param("active") boolean active);
```

### Indexing

```java
@Entity
@Table(indexes = {
    @Index(name = "idx_email", columnList = "email"),
    @Index(name = "idx_name", columnList = "name")
})
public class User {
    // Entity fields
}
```

## Caching

### Enable Caching

```java
@Configuration
@EnableCaching
public class CacheConfig {
    
    @Bean
    public CacheManager cacheManager() {
        CaffeineCache usersCache = new CaffeineCache("users",
            Caffeine.newBuilder()
                .maximumSize(1000)
                .expireAfterWrite(10, TimeUnit.MINUTES)
                .build());
        
        SimpleCacheManager cacheManager = new SimpleCacheManager();
        cacheManager.setCaches(Arrays.asList(usersCache));
        return cacheManager;
    }
}
```

### Use Caching

```java
@Cacheable(value = "users", key = "#id")
public User getUserById(Long id) {
    return userRepository.findById(id).orElse(null);
}

@CacheEvict(value = "users", key = "#id")
public void deleteUser(Long id) {
    userRepository.deleteById(id);
}
```

## Async Processing

### Use Async

```java
@Configuration
@EnableAsync
public class AsyncConfig {
    
    @Bean
    public Executor taskExecutor() {
        ThreadPoolTaskExecutor executor = new ThreadPoolTaskExecutor();
        executor.setCorePoolSize(10);
        executor.setMaxPoolSize(20);
        executor.setQueueCapacity(100);
        executor.initialize();
        return executor;
    }
}

@Service
public class EmailService {
    
    @Async
    public CompletableFuture<Void> sendEmail(String to, String subject, String body) {
        // Email sending logic
        return CompletableFuture.completedFuture(null);
    }
}
```

## Connection Pooling

### HTTP Client Pooling

```java
@Configuration
public class HttpClientConfig {
    
    @Bean
    public RestTemplate restTemplate() {
        HttpComponentsClientHttpRequestFactory factory = 
            new HttpComponentsClientHttpRequestFactory();
        factory.setConnectTimeout(5000);
        factory.setReadTimeout(10000);
        
        PoolingHttpClientConnectionManager connectionManager = 
            new PoolingHttpClientConnectionManager();
        connectionManager.setMaxTotal(100);
        connectionManager.setDefaultMaxPerRoute(20);
        
        CloseableHttpClient httpClient = HttpClients.custom()
            .setConnectionManager(connectionManager)
            .build();
        
        factory.setHttpClient(httpClient);
        return new RestTemplate(factory);
    }
}
```

## JVM Tuning

### JVM Options

```bash
java -Xms512m -Xmx2g \
     -XX:+UseG1GC \
     -XX:MaxGCPauseMillis=200 \
     -XX:+UseStringDeduplication \
     -jar application.jar
```

## Monitoring

### Actuator Metrics

```properties
management.endpoints.web.exposure.include=metrics,health
management.metrics.export.prometheus.enabled=true
```

### Custom Metrics

```java
@Service
public class UserService {
    
    private final Counter userCounter;
    private final Timer userTimer;
    
    public UserService(MeterRegistry meterRegistry) {
        this.userCounter = Counter.builder("users.created")
            .description("Number of users created")
            .register(meterRegistry);
        
        this.userTimer = Timer.builder("user.operation.time")
            .description("Time taken for user operations")
            .register(meterRegistry);
    }
    
    public User createUser(User user) {
        Timer.Sample sample = Timer.start();
        try {
            User savedUser = userRepository.save(user);
            userCounter.increment();
            return savedUser;
        } finally {
            sample.stop(userTimer);
        }
    }
}
```

## Best Practices

1. **Use Connection Pooling**: Configure appropriate pool sizes
2. **Enable Caching**: Cache frequently accessed data
3. **Use Async**: Use async for long-running operations
4. **Optimize Queries**: Use projections and pagination
5. **Monitor Performance**: Use metrics and monitoring
6. **Tune JVM**: Configure JVM options appropriately
7. **Use CDN**: Use CDN for static resources
8. **Compress Responses**: Enable response compression

## Summary

Spring Boot Performance Optimization:
- Database connection pooling
- Caching strategies
- Async processing
- Query optimization
- JVM tuning
- Essential for high-performance applications


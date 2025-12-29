# Spring Boot Rate Limiting

## What is Rate Limiting?

Rate limiting is a technique to control the rate of requests a client can make to an API. It helps prevent abuse and ensures fair usage of resources.

## Bucket4j Rate Limiting

### Add Dependency

```xml
<dependency>
    <groupId>com.bucket4j</groupId>
    <artifactId>bucket4j-core</artifactId>
    <version>8.7.0</version>
</dependency>
<dependency>
    <groupId>com.bucket4j</groupId>
    <artifactId>bucket4j-redis</artifactId>
    <version>8.7.0</version>
</dependency>
```

### Rate Limiting Filter

```java
@Component
@Order(1)
public class RateLimitingFilter implements Filter {
    
    private final Map<String, Bucket> cache = new ConcurrentHashMap<>();
    
    @Override
    public void doFilter(ServletRequest request, ServletResponse response, 
                        FilterChain chain) throws IOException, ServletException {
        HttpServletRequest httpRequest = (HttpServletRequest) request;
        String clientId = getClientId(httpRequest);
        
        Bucket bucket = resolveBucket(clientId);
        
        if (bucket.tryConsume(1)) {
            chain.doFilter(request, response);
        } else {
            HttpServletResponse httpResponse = (HttpServletResponse) response;
            httpResponse.setStatus(HttpStatus.TOO_MANY_REQUESTS.value());
            httpResponse.getWriter().write("Rate limit exceeded");
        }
    }
    
    private Bucket resolveBucket(String clientId) {
        return cache.computeIfAbsent(clientId, this::newBucket);
    }
    
    private Bucket newBucket(String clientId) {
        return Bucket4j.builder()
            .addLimit(Bandwidth.classic(10, Refill.intervally(10, Duration.ofMinutes(1))))
            .build();
    }
    
    private String getClientId(HttpServletRequest request) {
        String clientId = request.getHeader("X-Client-Id");
        if (clientId == null) {
            clientId = request.getRemoteAddr();
        }
        return clientId;
    }
}
```

## Redis-Based Rate Limiting

### Redis Rate Limiter

```java
@Service
public class RedisRateLimiter {
    
    private final RedisTemplate<String, String> redisTemplate;
    
    public RedisRateLimiter(RedisTemplate<String, String> redisTemplate) {
        this.redisTemplate = redisTemplate;
    }
    
    public boolean isAllowed(String key, int maxRequests, Duration duration) {
        String countKey = "rate_limit:" + key;
        String count = redisTemplate.opsForValue().get(countKey);
        
        if (count == null) {
            redisTemplate.opsForValue().set(countKey, "1", duration);
            return true;
        }
        
        int currentCount = Integer.parseInt(count);
        if (currentCount < maxRequests) {
            redisTemplate.opsForValue().increment(countKey);
            return true;
        }
        
        return false;
    }
}
```

## Annotation-Based Rate Limiting

### Custom Annotation

```java
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.RUNTIME)
public @interface RateLimit {
    int value() default 10;
    int duration() default 60; // seconds
}
```

### Rate Limit Interceptor

```java
@Component
public class RateLimitInterceptor implements HandlerInterceptor {
    
    private final RedisRateLimiter rateLimiter;
    
    public RateLimitInterceptor(RedisRateLimiter rateLimiter) {
        this.rateLimiter = rateLimiter;
    }
    
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, 
                           Object handler) throws Exception {
        if (handler instanceof HandlerMethod) {
            HandlerMethod handlerMethod = (HandlerMethod) handler;
            RateLimit rateLimit = handlerMethod.getMethodAnnotation(RateLimit.class);
            
            if (rateLimit != null) {
                String clientId = getClientId(request);
                String key = clientId + ":" + request.getRequestURI();
                
                if (!rateLimiter.isAllowed(key, rateLimit.value(), 
                                          Duration.ofSeconds(rateLimit.duration()))) {
                    response.setStatus(HttpStatus.TOO_MANY_REQUESTS.value());
                    response.getWriter().write("Rate limit exceeded");
                    return false;
                }
            }
        }
        
        return true;
    }
    
    private String getClientId(HttpServletRequest request) {
        String clientId = request.getHeader("X-Client-Id");
        return clientId != null ? clientId : request.getRemoteAddr();
    }
}
```

### Using Annotation

```java
@RestController
@RequestMapping("/api/users")
public class UserController {
    
    @GetMapping
    @RateLimit(value = 100, duration = 60)
    public ResponseEntity<List<User>> getAllUsers() {
        return ResponseEntity.ok(userService.getAllUsers());
    }
    
    @PostMapping
    @RateLimit(value = 10, duration = 60)
    public ResponseEntity<User> createUser(@RequestBody User user) {
        return ResponseEntity.ok(userService.createUser(user));
    }
}
```

## Global Rate Limiting

### Global Rate Limiter

```java
@Component
public class GlobalRateLimiter {
    
    private final Map<String, RateLimiter> limiters = new ConcurrentHashMap<>();
    
    public boolean isAllowed(String key, int permitsPerSecond) {
        RateLimiter limiter = limiters.computeIfAbsent(key, 
            k -> RateLimiter.create(permitsPerSecond));
        return limiter.tryAcquire();
    }
}
```

## Best Practices

1. **Use Redis**: Use Redis for distributed rate limiting
2. **Different Limits**: Apply different limits for different endpoints
3. **Client Identification**: Properly identify clients
4. **Return Headers**: Return rate limit headers in responses
5. **Monitor**: Monitor rate limit violations

## Summary

Spring Boot Rate Limiting:
- Prevents API abuse
- Ensures fair resource usage
- Supports various strategies
- Essential for production APIs


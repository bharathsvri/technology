# Spring Boot Custom Annotations

## Creating Custom Annotations

Custom annotations allow you to add metadata to your code and create reusable, declarative solutions for common patterns.

## Method-Level Annotations

### Execution Time Annotation

```java
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.RUNTIME)
public @interface LogExecutionTime {
    String value() default "";
}
```

### AOP Aspect

```java
@Aspect
@Component
public class LoggingAspect {
    
    private static final Logger logger = LoggerFactory.getLogger(LoggingAspect.class);
    
    @Around("@annotation(LogExecutionTime)")
    public Object logExecutionTime(ProceedingJoinPoint joinPoint) throws Throwable {
        long start = System.currentTimeMillis();
        
        Object proceed = joinPoint.proceed();
        
        long executionTime = System.currentTimeMillis() - start;
        logger.info("{} executed in {}ms", joinPoint.getSignature(), executionTime);
        
        return proceed;
    }
}
```

### Using Annotation

```java
@Service
public class UserService {
    
    @LogExecutionTime
    public User getUserById(Long id) {
        return userRepository.findById(id).orElse(null);
    }
}
```

## Validation Annotations

### Custom Validator

```java
@Target({ElementType.FIELD})
@Retention(RetentionPolicy.RUNTIME)
@Constraint(validatedBy = PhoneNumberValidator.class)
public @interface ValidPhoneNumber {
    String message() default "Invalid phone number";
    Class<?>[] groups() default {};
    Class<? extends Payload>[] payload() default {};
}
```

### Validator Implementation

```java
public class PhoneNumberValidator implements ConstraintValidator<ValidPhoneNumber, String> {
    
    @Override
    public void initialize(ValidPhoneNumber constraintAnnotation) {
    }
    
    @Override
    public boolean isValid(String phoneNumber, ConstraintValidatorContext context) {
        if (phoneNumber == null) {
            return true;
        }
        return phoneNumber.matches("^[0-9]{10}$");
    }
}
```

### Using Validator

```java
public class User {
    @ValidPhoneNumber
    private String phoneNumber;
}
```

## Security Annotations

### Role Required Annotation

```java
@Target({ElementType.METHOD, ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
public @interface RequireRole {
    String[] value();
}
```

### Security Aspect

```java
@Aspect
@Component
public class SecurityAspect {
    
    @Autowired
    private SecurityContext securityContext;
    
    @Around("@annotation(RequireRole)")
    public Object checkRole(ProceedingJoinPoint joinPoint) throws Throwable {
        RequireRole requireRole = ((MethodSignature) joinPoint.getSignature())
            .getMethod()
            .getAnnotation(RequireRole.class);
        
        String[] requiredRoles = requireRole.value();
        Authentication authentication = securityContext.getAuthentication();
        
        if (authentication == null) {
            throw new AuthenticationException("Not authenticated");
        }
        
        boolean hasRole = Arrays.stream(requiredRoles)
            .anyMatch(role -> authentication.getAuthorities().stream()
                .anyMatch(a -> a.getAuthority().equals("ROLE_" + role)));
        
        if (!hasRole) {
            throw new AccessDeniedException("Insufficient permissions");
        }
        
        return joinPoint.proceed();
    }
}
```

## Caching Annotations

### Custom Cache Annotation

```java
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.RUNTIME)
public @interface CacheResult {
    String value();
    long ttl() default 3600; // seconds
}
```

### Cache Aspect

```java
@Aspect
@Component
public class CacheAspect {
    
    private final CacheManager cacheManager;
    
    @Around("@annotation(CacheResult)")
    public Object cacheResult(ProceedingJoinPoint joinPoint) throws Throwable {
        CacheResult cacheResult = ((MethodSignature) joinPoint.getSignature())
            .getMethod()
            .getAnnotation(CacheResult.class);
        
        String cacheName = cacheResult.value();
        String key = generateKey(joinPoint);
        
        Cache cache = cacheManager.getCache(cacheName);
        Cache.ValueWrapper wrapper = cache.get(key);
        
        if (wrapper != null) {
            return wrapper.get();
        }
        
        Object result = joinPoint.proceed();
        cache.put(key, result);
        
        return result;
    }
    
    private String generateKey(ProceedingJoinPoint joinPoint) {
        return joinPoint.getSignature().getName() + 
               Arrays.toString(joinPoint.getArgs());
    }
}
```

## Transaction Annotations

### Custom Transaction Annotation

```java
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.RUNTIME)
public @interface TransactionalReadOnly {
    boolean readOnly() default true;
}
```

### Transaction Aspect

```java
@Aspect
@Component
public class TransactionAspect {
    
    @Autowired
    private PlatformTransactionManager transactionManager;
    
    @Around("@annotation(TransactionalReadOnly)")
    public Object handleTransaction(ProceedingJoinPoint joinPoint) throws Throwable {
        TransactionalReadOnly annotation = ((MethodSignature) joinPoint.getSignature())
            .getMethod()
            .getAnnotation(TransactionalReadOnly.class);
        
        DefaultTransactionDefinition definition = new DefaultTransactionDefinition();
        definition.setReadOnly(annotation.readOnly());
        
        TransactionStatus status = transactionManager.getTransaction(definition);
        
        try {
            Object result = joinPoint.proceed();
            transactionManager.commit(status);
            return result;
        } catch (Exception e) {
            transactionManager.rollback(status);
            throw e;
        }
    }
}
```

## Best Practices

1. **Clear Purpose**: Each annotation should have a clear purpose
2. **Documentation**: Document annotation usage
3. **Reusability**: Make annotations reusable
4. **Performance**: Consider performance implications
5. **Testing**: Test annotation behavior

## Summary

Spring Boot Custom Annotations:
- Create reusable patterns
- Reduce boilerplate code
- Improve code readability
- Essential for clean code architecture


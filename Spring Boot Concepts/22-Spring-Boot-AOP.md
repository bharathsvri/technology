# Spring Boot AOP

## What is AOP?

Aspect-Oriented Programming (AOP) complements Object-Oriented Programming by providing another way of thinking about program structure. It allows you to add cross-cutting concerns like logging, security, and transactions.

## Setting Up AOP

### Add Dependency

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-aop</artifactId>
</dependency>
```

### Enable AOP

```java
@SpringBootApplication
@EnableAspectJAutoProxy
public class Application {
    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}
```

## AOP Concepts

### Aspect
A module that encapsulates cross-cutting concerns.

### Join Point
A point during execution of a program, such as method execution.

### Pointcut
A predicate that matches join points.

### Advice
Action taken at a join point.

### Weaving
Process of applying aspects to target objects.

## Types of Advice

### @Before

```java
@Aspect
@Component
public class LoggingAspect {
    
    @Before("execution(* com.example.service.*.*(..))")
    public void logBefore(JoinPoint joinPoint) {
        System.out.println("Before method: " + joinPoint.getSignature().getName());
    }
}
```

### @After

```java
@After("execution(* com.example.service.*.*(..))")
public void logAfter(JoinPoint joinPoint) {
    System.out.println("After method: " + joinPoint.getSignature().getName());
}
```

### @AfterReturning

```java
@AfterReturning(pointcut = "execution(* com.example.service.*.*(..))", 
                returning = "result")
public void logAfterReturning(JoinPoint joinPoint, Object result) {
    System.out.println("Method returned: " + result);
}
```

### @AfterThrowing

```java
@AfterThrowing(pointcut = "execution(* com.example.service.*.*(..))", 
               throwing = "error")
public void logAfterThrowing(JoinPoint joinPoint, Throwable error) {
    System.out.println("Exception: " + error.getMessage());
}
```

### @Around

```java
@Around("execution(* com.example.service.*.*(..))")
public Object logAround(ProceedingJoinPoint joinPoint) throws Throwable {
    System.out.println("Before: " + joinPoint.getSignature().getName());
    
    try {
        Object result = joinPoint.proceed();
        System.out.println("After: " + joinPoint.getSignature().getName());
        return result;
    } catch (Exception e) {
        System.out.println("Exception: " + e.getMessage());
        throw e;
    }
}
```

## Pointcut Expressions

### Execution Pointcut

```java
@Pointcut("execution(* com.example.service.*.*(..))")
public void serviceMethods() {}
```

### Within Pointcut

```java
@Pointcut("within(com.example.service.*)")
public void inServiceLayer() {}
```

### Annotation Pointcut

```java
@Pointcut("@annotation(com.example.annotation.LogExecutionTime)")
public void logExecutionTimeMethods() {}
```

### Combining Pointcuts

```java
@Pointcut("execution(* com.example.service.*.*(..)) && @annotation(Loggable)")
public void loggableServiceMethods() {}
```

## Custom Annotations

### Creating Custom Annotation

```java
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.RUNTIME)
public @interface LogExecutionTime {
}
```

### Using Custom Annotation

```java
@Aspect
@Component
public class LoggingAspect {
    
    @Around("@annotation(LogExecutionTime)")
    public Object logExecutionTime(ProceedingJoinPoint joinPoint) throws Throwable {
        long start = System.currentTimeMillis();
        
        Object proceed = joinPoint.proceed();
        
        long executionTime = System.currentTimeMillis() - start;
        System.out.println(joinPoint.getSignature() + " executed in " + 
                         executionTime + "ms");
        
        return proceed;
    }
}
```

### Using in Service

```java
@Service
public class UserService {
    
    @LogExecutionTime
    public User getUserById(Long id) {
        return userRepository.findById(id).orElse(null);
    }
}
```

## Complete Example

### Logging Aspect

```java
@Aspect
@Component
public class LoggingAspect {
    
    private static final Logger logger = LoggerFactory.getLogger(LoggingAspect.class);
    
    @Pointcut("execution(* com.example.service.*.*(..))")
    public void serviceMethods() {}
    
    @Before("serviceMethods()")
    public void logBefore(JoinPoint joinPoint) {
        logger.info("Entering method: {} with arguments: {}", 
                   joinPoint.getSignature().getName(), 
                   Arrays.toString(joinPoint.getArgs()));
    }
    
    @AfterReturning(pointcut = "serviceMethods()", returning = "result")
    public void logAfterReturning(JoinPoint joinPoint, Object result) {
        logger.info("Method {} returned: {}", 
                   joinPoint.getSignature().getName(), result);
    }
    
    @AfterThrowing(pointcut = "serviceMethods()", throwing = "error")
    public void logAfterThrowing(JoinPoint joinPoint, Throwable error) {
        logger.error("Exception in method: {}", 
                    joinPoint.getSignature().getName(), error);
    }
}
```

### Performance Monitoring Aspect

```java
@Aspect
@Component
public class PerformanceAspect {
    
    @Around("@annotation(LogExecutionTime)")
    public Object measureExecutionTime(ProceedingJoinPoint joinPoint) throws Throwable {
        long start = System.currentTimeMillis();
        
        try {
            return joinPoint.proceed();
        } finally {
            long executionTime = System.currentTimeMillis() - start;
            System.out.println(joinPoint.getSignature() + " executed in " + 
                             executionTime + "ms");
        }
    }
}
```

## Best Practices

1. **Use Specific Pointcuts**: Be specific with pointcut expressions
2. **Keep Aspects Focused**: Each aspect should handle one concern
3. **Use Custom Annotations**: Use annotations for better control
4. **Handle Exceptions**: Properly handle exceptions in aspects
5. **Avoid Business Logic**: Don't put business logic in aspects
6. **Document Aspects**: Document what each aspect does

## Summary

Spring Boot AOP:
- Enables cross-cutting concerns
- Reduces code duplication
- Improves code organization
- Essential for logging, security, and monitoring


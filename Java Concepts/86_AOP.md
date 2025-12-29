# AOP (Aspect-Oriented Programming)

## What is AOP?
AOP allows separation of cross-cutting concerns (logging, security, transactions) from business logic.

## Spring AOP

### Aspect
```java
import org.aspectj.lang.annotation.*;
import org.springframework.stereotype.Component;

@Aspect
@Component
public class LoggingAspect {
    
    @Before("execution(* com.example.service.*.*(..))")
    public void logBefore(JoinPoint joinPoint) {
        System.out.println("Before: " + joinPoint.getSignature().getName());
    }
    
    @After("execution(* com.example.service.*.*(..))")
    public void logAfter(JoinPoint joinPoint) {
        System.out.println("After: " + joinPoint.getSignature().getName());
    }
    
    @AfterReturning(pointcut = "execution(* com.example.service.*.*(..))", 
                   returning = "result")
    public void logAfterReturning(JoinPoint joinPoint, Object result) {
        System.out.println("Returned: " + result);
    }
    
    @AfterThrowing(pointcut = "execution(* com.example.service.*.*(..))", 
                  throwing = "error")
    public void logAfterThrowing(JoinPoint joinPoint, Throwable error) {
        System.out.println("Exception: " + error.getMessage());
    }
    
    @Around("execution(* com.example.service.*.*(..))")
    public Object logAround(ProceedingJoinPoint joinPoint) throws Throwable {
        long start = System.currentTimeMillis();
        Object result = joinPoint.proceed();
        long duration = System.currentTimeMillis() - start;
        System.out.println("Duration: " + duration + "ms");
        return result;
    }
}
```

## Pointcut Expressions
```java
@Aspect
@Component
public class PointcutAspect {
    
    // All methods in service package
    @Pointcut("execution(* com.example.service.*.*(..))")
    public void serviceMethods() {}
    
    // Methods with @Transactional
    @Pointcut("@annotation(org.springframework.transaction.annotation.Transactional)")
    public void transactionalMethods() {}
    
    // Methods in classes with @Service
    @Pointcut("@within(org.springframework.stereotype.Service)")
    public void serviceClasses() {}
    
    // Combine pointcuts
    @Before("serviceMethods() && transactionalMethods()")
    public void logServiceTransaction() {
        System.out.println("Service transaction method");
    }
}
```

## Custom Annotations
```java
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.RUNTIME)
public @interface LogExecutionTime {
}

@Aspect
@Component
public class ExecutionTimeAspect {
    
    @Around("@annotation(LogExecutionTime)")
    public Object logExecutionTime(ProceedingJoinPoint joinPoint) throws Throwable {
        long start = System.currentTimeMillis();
        Object result = joinPoint.proceed();
        long duration = System.currentTimeMillis() - start;
        System.out.println(joinPoint.getSignature() + " executed in " + duration + "ms");
        return result;
    }
}

// Usage
@Service
public class UserService {
    @LogExecutionTime
    public User findUser(Long id) {
        return userRepository.findById(id);
    }
}
```

## Best Practices
1. Use AOP for cross-cutting concerns
2. Keep aspects focused
3. Use appropriate pointcuts
4. Avoid overusing AOP
5. Test aspects separately
6. Document aspect behavior
7. Consider performance impact
8. Use @Order for multiple aspects


# Spring Boot Async

## What is Asynchronous Processing?

Asynchronous processing allows methods to execute in separate threads, improving application performance by not blocking the main thread while waiting for long-running operations.

## Setting Up Async

### Enable Async

```java
@SpringBootApplication
@EnableAsync
public class Application {
    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}
```

## Using @Async

### Basic Async Method

```java
@Service
public class EmailService {
    
    @Async
    public void sendEmail(String to, String subject, String body) {
        // Long-running email sending operation
        System.out.println("Sending email to: " + to);
        // Simulate email sending
        try {
            Thread.sleep(2000);
        } catch (InterruptedException e) {
            Thread.currentThread().interrupt();
        }
        System.out.println("Email sent to: " + to);
    }
}
```

### Async with Return Value

```java
@Service
public class UserService {
    
    @Async
    public CompletableFuture<User> getUserAsync(Long id) {
        // Simulate database call
        try {
            Thread.sleep(1000);
        } catch (InterruptedException e) {
            Thread.currentThread().interrupt();
        }
        User user = userRepository.findById(id).orElse(null);
        return CompletableFuture.completedFuture(user);
    }
}
```

## Custom Executor

### Thread Pool Configuration

```java
@Configuration
@EnableAsync
public class AsyncConfig implements AsyncConfigurer {
    
    @Override
    @Bean(name = "taskExecutor")
    public Executor getAsyncExecutor() {
        ThreadPoolTaskExecutor executor = new ThreadPoolTaskExecutor();
        executor.setCorePoolSize(5);
        executor.setMaxPoolSize(10);
        executor.setQueueCapacity(100);
        executor.setThreadNamePrefix("async-");
        executor.initialize();
        return executor;
    }
    
    @Override
    public AsyncUncaughtExceptionHandler getAsyncUncaughtExceptionHandler() {
        return new CustomAsyncExceptionHandler();
    }
}
```

### Using Custom Executor

```java
@Service
public class ReportService {
    
    @Async("taskExecutor")
    public CompletableFuture<String> generateReport() {
        // Report generation logic
        return CompletableFuture.completedFuture("Report generated");
    }
}
```

## Exception Handling

### Custom Exception Handler

```java
public class CustomAsyncExceptionHandler implements AsyncUncaughtExceptionHandler {
    
    @Override
    public void handleUncaughtException(Throwable throwable, Method method, Object... params) {
        System.err.println("Exception in async method: " + method.getName());
        System.err.println("Exception: " + throwable.getMessage());
    }
}
```

### Handling Exceptions in CompletableFuture

```java
@Async
public CompletableFuture<String> processData() {
    return CompletableFuture.supplyAsync(() -> {
        // Processing logic
        if (error) {
            throw new RuntimeException("Processing error");
        }
        return "Success";
    }).exceptionally(ex -> {
        System.err.println("Error: " + ex.getMessage());
        return "Error occurred";
    });
}
```

## Multiple Async Methods

### Parallel Execution

```java
@Service
public class DataProcessingService {
    
    @Async
    public CompletableFuture<String> processData1() {
        // Process data 1
        return CompletableFuture.completedFuture("Data 1 processed");
    }
    
    @Async
    public CompletableFuture<String> processData2() {
        // Process data 2
        return CompletableFuture.completedFuture("Data 2 processed");
    }
    
    public void processAll() {
        CompletableFuture<String> future1 = processData1();
        CompletableFuture<String> future2 = processData2();
        
        CompletableFuture.allOf(future1, future2).join();
    }
}
```

## Best Practices

1. **Use Appropriate Thread Pool**: Configure thread pool size based on workload
2. **Handle Exceptions**: Always handle exceptions in async methods
3. **Use CompletableFuture**: Use CompletableFuture for return values
4. **Avoid Blocking**: Don't block async methods unnecessarily
5. **Monitor Performance**: Monitor thread pool usage

## Summary

Spring Boot Async:
- Enables asynchronous method execution
- Improves application performance
- Supports custom executors
- Essential for long-running operations


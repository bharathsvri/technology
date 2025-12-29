# Spring Boot Application Events

## What are Application Events?

Application events allow components to communicate in a loosely coupled way. Spring Boot provides a built-in event system for publishing and listening to application events.

## Built-in Events

### ApplicationReadyEvent

Fired when the application is ready to serve requests:

```java
@Component
public class ApplicationStartupListener {
    
    @EventListener
    public void handleApplicationReady(ApplicationReadyEvent event) {
        System.out.println("Application is ready!");
    }
}
```

### ContextRefreshedEvent

Fired when the application context is refreshed:

```java
@Component
public class ContextRefreshListener {
    
    @EventListener
    public void handleContextRefresh(ContextRefreshedEvent event) {
        System.out.println("Context refreshed");
    }
}
```

## Custom Events

### Creating Custom Event

```java
public class UserCreatedEvent extends ApplicationEvent {
    private final User user;
    
    public UserCreatedEvent(Object source, User user) {
        super(source);
        this.user = user;
    }
    
    public User getUser() {
        return user;
    }
}
```

### Publishing Events

```java
@Service
public class UserService {
    
    private final ApplicationEventPublisher eventPublisher;
    
    public UserService(ApplicationEventPublisher eventPublisher) {
        this.eventPublisher = eventPublisher;
    }
    
    public User createUser(User user) {
        User savedUser = userRepository.save(user);
        eventPublisher.publishEvent(new UserCreatedEvent(this, savedUser));
        return savedUser;
    }
}
```

### Listening to Events

```java
@Component
public class UserEventListener {
    
    @EventListener
    public void handleUserCreated(UserCreatedEvent event) {
        User user = event.getUser();
        System.out.println("User created: " + user.getName());
        // Send welcome email, create audit log, etc.
    }
    
    @Async
    @EventListener
    public void handleUserCreatedAsync(UserCreatedEvent event) {
        // Async processing
    }
}
```

## Conditional Event Listening

### Using @ConditionalOnProperty

```java
@Component
@ConditionalOnProperty(name = "events.email.enabled", havingValue = "true")
public class EmailEventListener {
    
    @EventListener
    public void handleUserCreated(UserCreatedEvent event) {
        emailService.sendWelcomeEmail(event.getUser());
    }
}
```

### Using SpEL

```java
@EventListener(condition = "#event.user.email != null")
public void handleUserCreated(UserCreatedEvent event) {
    // Only handle if user has email
}
```

## Transaction-Bound Events

### Using @TransactionalEventListener

```java
@Component
public class UserEventListener {
    
    @TransactionalEventListener(phase = TransactionPhase.AFTER_COMMIT)
    public void handleUserCreatedAfterCommit(UserCreatedEvent event) {
        // This runs after transaction commits
        emailService.sendWelcomeEmail(event.getUser());
    }
    
    @TransactionalEventListener(phase = TransactionPhase.AFTER_ROLLBACK)
    public void handleUserCreatedAfterRollback(UserCreatedEvent event) {
        // This runs if transaction rolls back
        System.out.println("Transaction rolled back for user: " + event.getUser().getId());
    }
}
```

## Multiple Listeners

### Ordering Listeners

```java
@Component
public class UserEventListener {
    
    @EventListener
    @Order(1)
    public void handleUserCreatedFirst(UserCreatedEvent event) {
        System.out.println("First listener");
    }
    
    @EventListener
    @Order(2)
    public void handleUserCreatedSecond(UserCreatedEvent event) {
        System.out.println("Second listener");
    }
}
```

## Complete Example

### Event Classes

```java
public class UserCreatedEvent extends ApplicationEvent {
    private final User user;
    
    public UserCreatedEvent(Object source, User user) {
        super(source);
        this.user = user;
    }
    
    public User getUser() {
        return user;
    }
}

public class UserUpdatedEvent extends ApplicationEvent {
    private final User user;
    
    public UserUpdatedEvent(Object source, User user) {
        super(source);
        this.user = user;
    }
    
    public User getUser() {
        return user;
    }
}
```

### Service

```java
@Service
public class UserService {
    
    private final ApplicationEventPublisher eventPublisher;
    private final UserRepository userRepository;
    
    public User createUser(User user) {
        User savedUser = userRepository.save(user);
        eventPublisher.publishEvent(new UserCreatedEvent(this, savedUser));
        return savedUser;
    }
    
    public User updateUser(Long id, User userDetails) {
        User user = userRepository.findById(id).orElseThrow();
        user.setName(userDetails.getName());
        User savedUser = userRepository.save(user);
        eventPublisher.publishEvent(new UserUpdatedEvent(this, savedUser));
        return savedUser;
    }
}
```

### Event Listeners

```java
@Component
public class UserEventListener {
    
    private final EmailService emailService;
    private final AuditService auditService;
    
    @EventListener
    @Async
    public void handleUserCreated(UserCreatedEvent event) {
        User user = event.getUser();
        emailService.sendWelcomeEmail(user);
        auditService.logUserCreation(user);
    }
    
    @TransactionalEventListener(phase = TransactionPhase.AFTER_COMMIT)
    public void handleUserUpdated(UserUpdatedEvent event) {
        auditService.logUserUpdate(event.getUser());
    }
}
```

## Best Practices

1. **Use for Loose Coupling**: Use events to decouple components
2. **Keep Events Simple**: Keep event classes simple
3. **Handle Errors**: Handle errors in event listeners
4. **Use Async**: Use async listeners for long-running operations
5. **Document Events**: Document when events are published

## Summary

Spring Boot Application Events:
- Enables loose coupling between components
- Supports custom events
- Can be synchronous or asynchronous
- Essential for event-driven architecture


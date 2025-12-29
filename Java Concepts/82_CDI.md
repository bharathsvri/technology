# CDI (Contexts and Dependency Injection)

## What is CDI?
CDI provides dependency injection and lifecycle management for Java applications.

## Basic Injection
```java
import javax.inject.*;

@ApplicationScoped
public class UserService {
    @Inject
    private UserRepository userRepository;
    
    public User findUser(Long id) {
        return userRepository.findById(id);
    }
}
```

## Scopes
```java
// Application scoped (singleton)
@ApplicationScoped
public class ApplicationService {
}

// Session scoped
@SessionScoped
public class SessionBean implements Serializable {
}

// Request scoped
@RequestScoped
public class RequestBean {
}

// Dependent (new instance each time)
@Dependent
public class DependentBean {
}
```

## Qualifiers
```java
@Qualifier
@Retention(RetentionPolicy.RUNTIME)
@Target({ElementType.TYPE, ElementType.METHOD, ElementType.FIELD, ElementType.PARAMETER})
public @interface Database {
}

@Database
@ApplicationScoped
public class DatabaseUserRepository implements UserRepository {
}

@Inject
@Database
private UserRepository userRepository;
```

## Producers
```java
@ApplicationScoped
public class ConfigurationProducer {
    
    @Produces
    @Named("databaseUrl")
    public String getDatabaseUrl() {
        return System.getProperty("db.url", "jdbc:mysql://localhost:3306/mydb");
    }
    
    @Produces
    @RequestScoped
    public EntityManager createEntityManager() {
        return Persistence.createEntityManagerFactory("myPU").createEntityManager();
    }
}
```

## Events
```java
// Event producer
@ApplicationScoped
public class UserService {
    @Inject
    private Event<UserCreatedEvent> userCreatedEvent;
    
    public void createUser(User user) {
        // Create user
        userCreatedEvent.fire(new UserCreatedEvent(user));
    }
}

// Event observer
@ApplicationScoped
public class EmailService {
    @Observes
    public void sendWelcomeEmail(UserCreatedEvent event) {
        // Send email
    }
}
```

## Interceptors
```java
@InterceptorBinding
@Target({ElementType.TYPE, ElementType.METHOD})
@Retention(RetentionPolicy.RUNTIME)
public @interface Logged {
}

@Logged
@Interceptor
public class LoggingInterceptor {
    @AroundInvoke
    public Object log(InvocationContext context) throws Exception {
        System.out.println("Method: " + context.getMethod().getName());
        return context.proceed();
    }
}
```

## Best Practices
1. Use appropriate scopes
2. Prefer constructor injection
3. Use qualifiers for multiple implementations
4. Use events for loose coupling
5. Use interceptors for cross-cutting concerns
6. Handle lifecycle properly
7. Use producers for complex objects
8. Test with CDI test framework


# Spring Framework Basics

## What is Spring?
Spring is a comprehensive framework for enterprise Java development, providing dependency injection, AOP, and more.

## Dependency Injection

### Component Annotation
```java
import org.springframework.stereotype.Component;

@Component
public class UserService {
    public void saveUser(User user) {
        // Save user logic
    }
}
```

### Service Annotation
```java
import org.springframework.stereotype.Service;

@Service
public class UserService {
    private UserRepository userRepository;
    
    @Autowired
    public UserService(UserRepository userRepository) {
        this.userRepository = userRepository;
    }
    
    public User findUser(Long id) {
        return userRepository.findById(id);
    }
}
```

### Repository Annotation
```java
import org.springframework.stereotype.Repository;

@Repository
public class UserRepository {
    public User findById(Long id) {
        // Database query
        return user;
    }
}
```

## Configuration

### Java Configuration
```java
import org.springframework.context.annotation.*;

@Configuration
@ComponentScan("com.example")
public class AppConfig {
    
    @Bean
    public DataSource dataSource() {
        // Configure data source
        return new HikariDataSource();
    }
    
    @Bean
    public EntityManagerFactory entityManagerFactory() {
        // Configure JPA
        return Persistence.createEntityManagerFactory("my-persistence-unit");
    }
}
```

### Application Context
```java
import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;

ApplicationContext context = 
    new AnnotationConfigApplicationContext(AppConfig.class);
UserService userService = context.getBean(UserService.class);
```

## Spring Boot Basics

### Main Application
```java
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class Application {
    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}
```

### REST Controller
```java
import org.springframework.web.bind.annotation.*;

@RestController
@RequestMapping("/api/users")
public class UserController {
    
    private final UserService userService;
    
    public UserController(UserService userService) {
        this.userService = userService;
    }
    
    @GetMapping
    public List<User> getAllUsers() {
        return userService.findAll();
    }
    
    @GetMapping("/{id}")
    public User getUser(@PathVariable Long id) {
        return userService.findById(id);
    }
    
    @PostMapping
    public User createUser(@RequestBody User user) {
        return userService.save(user);
    }
    
    @PutMapping("/{id}")
    public User updateUser(@PathVariable Long id, @RequestBody User user) {
        return userService.update(id, user);
    }
    
    @DeleteMapping("/{id}")
    public void deleteUser(@PathVariable Long id) {
        userService.delete(id);
    }
}
```

## Application Properties
```properties
# application.properties
spring.datasource.url=jdbc:mysql://localhost:3306/mydb
spring.datasource.username=root
spring.datasource.password=password
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver

spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
spring.jpa.properties.hibernate.format_sql=true
```

## Best Practices
1. Use constructor injection
2. Prefer @ComponentScan
3. Use @Configuration for Java config
4. Follow naming conventions
5. Use profiles for different environments
6. Keep controllers thin
7. Use service layer for business logic
8. Handle exceptions properly
9. Use Spring Boot for new projects
10. Follow REST conventions


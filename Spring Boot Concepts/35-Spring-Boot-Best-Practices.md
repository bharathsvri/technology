# Spring Boot Best Practices

## General Best Practices

### 1. Project Structure

```
src/
├── main/
│   ├── java/
│   │   └── com/
│   │       └── example/
│   │           ├── config/
│   │           ├── controller/
│   │           ├── service/
│   │           ├── repository/
│   │           ├── model/
│   │           ├── dto/
│   │           ├── exception/
│   │           └── Application.java
│   └── resources/
│       ├── application.properties
│       └── application-dev.properties
└── test/
```

### 2. Use Constructor Injection

```java
// ✅ Good
@Service
public class UserService {
    private final UserRepository userRepository;
    
    public UserService(UserRepository userRepository) {
        this.userRepository = userRepository;
    }
}

// ❌ Bad
@Service
public class UserService {
    @Autowired
    private UserRepository userRepository;
}
```

### 3. Use @ConfigurationProperties

```java
@ConfigurationProperties(prefix = "app")
public class AppProperties {
    private String name;
    private String version;
    // Getters and setters
}
```

### 4. Use Profiles

```properties
# application-dev.properties
spring.datasource.url=jdbc:h2:mem:devdb

# application-prod.properties
spring.datasource.url=jdbc:mysql://prod-server:3306/proddb
```

### 5. Handle Exceptions Globally

```java
@RestControllerAdvice
public class GlobalExceptionHandler {
    
    @ExceptionHandler(ResourceNotFoundException.class)
    public ResponseEntity<ErrorResponse> handleResourceNotFound(ResourceNotFoundException ex) {
        // Handle exception
    }
}
```

### 6. Use DTOs

```java
public class UserDTO {
    private String name;
    private String email;
    // Getters and setters
}
```

### 7. Validate Input

```java
@PostMapping("/users")
public ResponseEntity<User> createUser(@Valid @RequestBody User user) {
    return ResponseEntity.ok(userService.createUser(user));
}
```

### 8. Use Transactions

```java
@Service
@Transactional
public class UserService {
    // Service methods are transactional
}
```

### 9. Use Logging

```java
@Service
public class UserService {
    private static final Logger logger = LoggerFactory.getLogger(UserService.class);
    
    public User getUser(Long id) {
        logger.debug("Getting user with id: {}", id);
        // Implementation
    }
}
```

### 10. Use Constants

```java
public class Constants {
    public static final String API_VERSION = "/api/v1";
    public static final int MAX_RETRY_ATTEMPTS = 3;
}
```

## Security Best Practices

### 1. Use Strong Passwords

```java
@Bean
public PasswordEncoder passwordEncoder() {
    return new BCryptPasswordEncoder();
}
```

### 2. Secure Sensitive Data

```properties
# Use environment variables
spring.datasource.password=${DB_PASSWORD}
```

### 3. Use HTTPS

```properties
server.ssl.key-store=classpath:keystore.p12
server.ssl.key-store-password=${KEYSTORE_PASSWORD}
```

## Performance Best Practices

### 1. Use Connection Pooling

```properties
spring.datasource.hikari.maximum-pool-size=10
spring.datasource.hikari.minimum-idle=5
```

### 2. Use Caching

```java
@Cacheable(value = "users", key = "#id")
public User getUser(Long id) {
    return userRepository.findById(id).orElse(null);
}
```

### 3. Use Async

```java
@Async
public CompletableFuture<String> processData() {
    // Long-running operation
}
```

### 4. Use Pagination

```java
@GetMapping("/users")
public Page<User> getUsers(Pageable pageable) {
    return userService.getAllUsers(pageable);
}
```

## Testing Best Practices

### 1. Write Unit Tests

```java
@ExtendWith(MockitoExtension.class)
class UserServiceTest {
    @Mock
    private UserRepository userRepository;
    
    @InjectMocks
    private UserService userService;
    
    @Test
    void testGetUser() {
        // Test implementation
    }
}
```

### 2. Write Integration Tests

```java
@SpringBootTest
class UserControllerIntegrationTest {
    @Autowired
    private MockMvc mockMvc;
    
    @Test
    void testGetUser() throws Exception {
        // Test implementation
    }
}
```

## Documentation Best Practices

### 1. Document APIs

```java
@Operation(summary = "Get user by ID")
@GetMapping("/{id}")
public ResponseEntity<User> getUser(@PathVariable Long id) {
    // Implementation
}
```

### 2. Use Meaningful Names

```java
// ✅ Good
public User getUserById(Long id)

// ❌ Bad
public User get(Long id)
```

## Summary

Spring Boot Best Practices:
- Follow consistent project structure
- Use constructor injection
- Handle exceptions globally
- Use DTOs and validation
- Implement proper security
- Optimize performance
- Write comprehensive tests
- Document your code


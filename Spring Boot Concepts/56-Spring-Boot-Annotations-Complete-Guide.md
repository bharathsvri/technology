# Spring Boot Annotations - Complete Guide

## Core Spring Boot Annotations

### @SpringBootApplication

**Purpose**: Main annotation that combines @Configuration, @EnableAutoConfiguration, and @ComponentScan.

**Example**:
```java
@SpringBootApplication
public class Application {
    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}
```

### @Configuration

**Purpose**: Indicates that a class declares one or more @Bean methods.

**Example**:
```java
@Configuration
public class AppConfig {
    @Bean
    public UserService userService() {
        return new UserService();
    }
}
```

### @Component

**Purpose**: Generic stereotype annotation indicating that an annotated class is a Spring component.

**Example**:
```java
@Component
public class UserService {
    // Service implementation
}
```

### @Service

**Purpose**: Specialization of @Component for service layer.

**Example**:
```java
@Service
public class UserService {
    public User getUser(Long id) {
        // Implementation
    }
}
```

### @Repository

**Purpose**: Specialization of @Component for data access layer.

**Example**:
```java
@Repository
public interface UserRepository extends JpaRepository<User, Long> {
    List<User> findByName(String name);
}
```

### @Controller

**Purpose**: Specialization of @Component for web controllers.

**Example**:
```java
@Controller
@RequestMapping("/users")
public class UserController {
    @GetMapping
    public String getAllUsers(Model model) {
        // Implementation
    }
}
```

### @RestController

**Purpose**: Combination of @Controller and @ResponseBody for REST controllers.

**Example**:
```java
@RestController
@RequestMapping("/api/users")
public class UserController {
    @GetMapping
    public List<User> getAllUsers() {
        return userService.getAllUsers();
    }
}
```

## Dependency Injection Annotations

### @Autowired

**Purpose**: Marks a constructor, field, setter method, or config method to be autowired.

**Example**:
```java
@Service
public class UserService {
    @Autowired
    private UserRepository userRepository;
    
    @Autowired
    public UserService(UserRepository userRepository) {
        this.userRepository = userRepository;
    }
}
```

### @Qualifier

**Purpose**: Used with @Autowired to specify which bean to inject when multiple beans of the same type exist.

**Example**:
```java
@Bean
@Qualifier("primary")
public DataSource primaryDataSource() {
    return new HikariDataSource();
}

@Bean
@Qualifier("secondary")
public DataSource secondaryDataSource() {
    return new HikariDataSource();
}

@Service
public class UserService {
    @Autowired
    @Qualifier("primary")
    private DataSource dataSource;
}
```

### @Required

**Purpose**: Indicates that a bean property must be populated at configuration time.

**Example**:
```java
public class UserService {
    @Required
    public void setUserRepository(UserRepository userRepository) {
        this.userRepository = userRepository;
    }
}
```

### @Value

**Purpose**: Used to inject values from properties files or environment variables.

**Example**:
```java
@Service
public class UserService {
    @Value("${app.name}")
    private String appName;
    
    @Value("${server.port:8080}")
    private int serverPort;
    
    @Value("#{systemProperties['user.name']}")
    private String systemUser;
}
```

## Bean Definition Annotations

### @Bean

**Purpose**: Indicates that a method produces a bean to be managed by Spring.

**Example**:
```java
@Configuration
public class AppConfig {
    @Bean
    public RestTemplate restTemplate() {
        return new RestTemplate();
    }
    
    @Bean(name = "customBean")
    public MyService myService() {
        return new MyService();
    }
}
```

### @Scope

**Purpose**: Specifies the scope of a bean.

**Example**:
```java
@Component
@Scope("prototype")
public class PrototypeBean {
    // Prototype instance
}

@Component
@Scope("singleton")
public class SingletonBean {
    // Singleton instance
}

@Component
@Scope("request")
public class RequestScopedBean {
    // Request-scoped instance
}

@Component
@Scope("session")
public class SessionScopedBean {
    // Session-scoped instance
}
```

### @Primary

**Purpose**: Indicates that a bean should be given preference when multiple candidates are qualified.

**Example**:
```java
@Bean
@Primary
public DataSource primaryDataSource() {
    return new HikariDataSource();
}

@Bean
public DataSource secondaryDataSource() {
    return new BasicDataSource();
}
```

### @Lazy

**Purpose**: Indicates that a bean should be lazily initialized.

**Example**:
```java
@Component
@Lazy
public class LazyBean {
    // Lazy initialization
}

@Bean
@Lazy
public ExpensiveService expensiveService() {
    return new ExpensiveService();
}
```

### @DependsOn

**Purpose**: Forces one or more beans to be initialized before the annotated bean.

**Example**:
```java
@Component
@DependsOn("databaseInitializer")
public class UserService {
    // Depends on databaseInitializer
}
```

## Web Annotations

### @RequestMapping

**Purpose**: Maps HTTP requests to handler methods.

**Example**:
```java
@Controller
@RequestMapping("/api/users")
public class UserController {
    @RequestMapping(method = RequestMethod.GET)
    public List<User> getAllUsers() {
        return userService.getAllUsers();
    }
    
    @RequestMapping(value = "/{id}", method = RequestMethod.GET)
    public User getUser(@PathVariable Long id) {
        return userService.getUserById(id);
    }
}
```

### @GetMapping

**Purpose**: Shortcut for @RequestMapping(method = RequestMethod.GET).

**Example**:
```java
@GetMapping("/users")
public List<User> getAllUsers() {
    return userService.getAllUsers();
}

@GetMapping("/users/{id}")
public User getUser(@PathVariable Long id) {
    return userService.getUserById(id);
}
```

### @PostMapping

**Purpose**: Shortcut for @RequestMapping(method = RequestMethod.POST).

**Example**:
```java
@PostMapping("/users")
public ResponseEntity<User> createUser(@RequestBody User user) {
    User created = userService.createUser(user);
    return ResponseEntity.status(HttpStatus.CREATED).body(created);
}
```

### @PutMapping

**Purpose**: Shortcut for @RequestMapping(method = RequestMethod.PUT).

**Example**:
```java
@PutMapping("/users/{id}")
public User updateUser(@PathVariable Long id, @RequestBody User user) {
    return userService.updateUser(id, user);
}
```

### @PatchMapping

**Purpose**: Shortcut for @RequestMapping(method = RequestMethod.PATCH).

**Example**:
```java
@PatchMapping("/users/{id}")
public User partialUpdateUser(@PathVariable Long id, @RequestBody Map<String, Object> updates) {
    return userService.partialUpdateUser(id, updates);
}
```

### @DeleteMapping

**Purpose**: Shortcut for @RequestMapping(method = RequestMethod.DELETE).

**Example**:
```java
@DeleteMapping("/users/{id}")
public ResponseEntity<Void> deleteUser(@PathVariable Long id) {
    userService.deleteUser(id);
    return ResponseEntity.noContent().build();
}
```

### @PathVariable

**Purpose**: Binds a method parameter to a URI template variable.

**Example**:
```java
@GetMapping("/users/{id}")
public User getUser(@PathVariable Long id) {
    return userService.getUserById(id);
}

@GetMapping("/users/{userId}/orders/{orderId}")
public Order getOrder(@PathVariable Long userId, @PathVariable Long orderId) {
    return orderService.getOrder(userId, orderId);
}

@GetMapping("/users/{id}")
public User getUser(@PathVariable("id") Long userId) {
    return userService.getUserById(userId);
}
```

### @RequestParam

**Purpose**: Binds a method parameter to a web request parameter.

**Example**:
```java
@GetMapping("/users")
public List<User> getUsers(
        @RequestParam(defaultValue = "0") int page,
        @RequestParam(defaultValue = "10") int size,
        @RequestParam(required = false) String name) {
    return userService.getUsers(page, size, name);
}

@GetMapping("/search")
public List<User> searchUsers(@RequestParam List<Long> ids) {
    return userService.getUsersByIds(ids);
}
```

### @RequestBody

**Purpose**: Binds the HTTP request body to a method parameter.

**Example**:
```java
@PostMapping("/users")
public User createUser(@RequestBody User user) {
    return userService.createUser(user);
}

@PostMapping("/users")
public User createUser(@Valid @RequestBody User user) {
    return userService.createUser(user);
}
```

### @ResponseBody

**Purpose**: Indicates that a method return value should be bound to the web response body.

**Example**:
```java
@Controller
public class UserController {
    @GetMapping("/users")
    @ResponseBody
    public List<User> getAllUsers() {
        return userService.getAllUsers();
    }
}
```

### @RequestHeader

**Purpose**: Binds a method parameter to a request header.

**Example**:
```java
@GetMapping("/users")
public List<User> getUsers(@RequestHeader("Authorization") String auth) {
    // Process authorization
    return userService.getAllUsers();
}

@GetMapping("/users")
public List<User> getUsers(
        @RequestHeader(value = "User-Agent", required = false) String userAgent) {
    return userService.getAllUsers();
}
```

### @CookieValue

**Purpose**: Binds a method parameter to an HTTP cookie value.

**Example**:
```java
@GetMapping("/profile")
public User getProfile(@CookieValue("sessionId") String sessionId) {
    return userService.getUserBySession(sessionId);
}
```

### @ModelAttribute

**Purpose**: Binds a method parameter or method return value to a named model attribute.

**Example**:
```java
@PostMapping("/users")
public String createUser(@ModelAttribute User user) {
    userService.createUser(user);
    return "redirect:/users";
}

@ModelAttribute("user")
public User getUser(@RequestParam(required = false) Long id) {
    if (id != null) {
        return userService.getUserById(id);
    }
    return new User();
}
```

## Validation Annotations

### @Valid

**Purpose**: Marks a property, method parameter, or method return type for validation.

**Example**:
```java
@PostMapping("/users")
public User createUser(@Valid @RequestBody User user) {
    return userService.createUser(user);
}
```

### @Validated

**Purpose**: Variant of @Valid for class-level validation.

**Example**:
```java
@Service
@Validated
public class UserService {
    public User createUser(@Valid User user) {
        return userRepository.save(user);
    }
}
```

### @NotNull

**Purpose**: Validates that the annotated element is not null.

**Example**:
```java
public class User {
    @NotNull(message = "Name cannot be null")
    private String name;
}
```

### @NotEmpty

**Purpose**: Validates that the annotated element is not null and not empty.

**Example**:
```java
public class User {
    @NotEmpty(message = "Email cannot be empty")
    private String email;
}
```

### @NotBlank

**Purpose**: Validates that the annotated string is not null, not empty, and not just whitespace.

**Example**:
```java
public class User {
    @NotBlank(message = "Username cannot be blank")
    private String username;
}
```

### @Size

**Purpose**: Validates the size of a collection, array, or string.

**Example**:
```java
public class User {
    @Size(min = 2, max = 50, message = "Name must be between 2 and 50 characters")
    private String name;
    
    @Size(min = 1, max = 10, message = "List must have between 1 and 10 items")
    private List<String> items;
}
```

### @Min / @Max

**Purpose**: Validates that the annotated value is greater than or equal to / less than or equal to the specified minimum/maximum.

**Example**:
```java
public class User {
    @Min(value = 18, message = "Age must be at least 18")
    @Max(value = 100, message = "Age must be at most 100")
    private Integer age;
}
```

### @Email

**Purpose**: Validates that the annotated string is a valid email address.

**Example**:
```java
public class User {
    @Email(message = "Email should be valid")
    private String email;
}
```

### @Pattern

**Purpose**: Validates that the annotated string matches the specified regular expression.

**Example**:
```java
public class User {
    @Pattern(regexp = "^[0-9]{10}$", message = "Phone number must be 10 digits")
    private String phoneNumber;
}
```

## JPA Annotations

### @Entity

**Purpose**: Specifies that the class is an entity.

**Example**:
```java
@Entity
@Table(name = "users")
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    private String name;
    private String email;
}
```

### @Table

**Purpose**: Specifies the primary table for the annotated entity.

**Example**:
```java
@Entity
@Table(name = "users", schema = "public", indexes = {
    @Index(name = "idx_email", columnList = "email")
})
public class User {
    // Entity fields
}
```

### @Id

**Purpose**: Specifies the primary key of an entity.

**Example**:
```java
@Entity
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
}
```

### @GeneratedValue

**Purpose**: Provides specification of generation strategies for the values of primary keys.

**Example**:
```java
@Entity
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    // Other strategies: AUTO, SEQUENCE, TABLE
}
```

### @Column

**Purpose**: Specifies the mapped column for a persistent property or field.

**Example**:
```java
@Entity
public class User {
    @Column(name = "user_name", nullable = false, length = 50, unique = true)
    private String name;
    
    @Column(updatable = false)
    private LocalDateTime createdAt;
}
```

### @OneToOne

**Purpose**: Defines a single-valued association to another entity.

**Example**:
```java
@Entity
public class User {
    @OneToOne(cascade = CascadeType.ALL)
    @JoinColumn(name = "address_id")
    private Address address;
}
```

### @OneToMany

**Purpose**: Defines a many-valued association with one-to-many multiplicity.

**Example**:
```java
@Entity
public class User {
    @OneToMany(mappedBy = "user", cascade = CascadeType.ALL)
    private List<Order> orders;
}
```

### @ManyToOne

**Purpose**: Defines a single-valued association to another entity class that has many-to-one multiplicity.

**Example**:
```java
@Entity
public class Order {
    @ManyToOne
    @JoinColumn(name = "user_id")
    private User user;
}
```

### @ManyToMany

**Purpose**: Defines a many-valued association with many-to-many multiplicity.

**Example**:
```java
@Entity
public class User {
    @ManyToMany
    @JoinTable(
        name = "user_roles",
        joinColumns = @JoinColumn(name = "user_id"),
        inverseJoinColumns = @JoinColumn(name = "role_id")
    )
    private Set<Role> roles;
}
```

### @JoinColumn

**Purpose**: Specifies a column for joining an entity association or element collection.

**Example**:
```java
@ManyToOne
@JoinColumn(name = "user_id", nullable = false)
private User user;
```

### @JoinTable

**Purpose**: Specifies the mapping of associations.

**Example**:
```java
@ManyToMany
@JoinTable(
    name = "user_roles",
    joinColumns = @JoinColumn(name = "user_id"),
    inverseJoinColumns = @JoinColumn(name = "role_id")
)
private Set<Role> roles;
```

## Transaction Annotations

### @Transactional

**Purpose**: Declares that a method or class is transactional.

**Example**:
```java
@Service
@Transactional
public class UserService {
    public User createUser(User user) {
        return userRepository.save(user);
    }
    
    @Transactional(readOnly = true)
    public User getUser(Long id) {
        return userRepository.findById(id).orElse(null);
    }
    
    @Transactional(propagation = Propagation.REQUIRES_NEW)
    public void logUser(User user) {
        logRepository.save(new Log("User created", user.getId()));
    }
}
```

## Caching Annotations

### @Cacheable

**Purpose**: Indicates that a method's result can be cached.

**Example**:
```java
@Cacheable(value = "users", key = "#id")
public User getUserById(Long id) {
    return userRepository.findById(id).orElse(null);
}

@Cacheable(value = "users", condition = "#id > 10")
public User getUser(Long id) {
    return userRepository.findById(id).orElse(null);
}
```

### @CacheEvict

**Purpose**: Indicates that a method should trigger cache eviction.

**Example**:
```java
@CacheEvict(value = "users", key = "#id")
public void deleteUser(Long id) {
    userRepository.deleteById(id);
}

@CacheEvict(value = "users", allEntries = true)
public void deleteAllUsers() {
    userRepository.deleteAll();
}
```

### @CachePut

**Purpose**: Indicates that a method should update the cache.

**Example**:
```java
@CachePut(value = "users", key = "#user.id")
public User updateUser(User user) {
    return userRepository.save(user);
}
```

### @Caching

**Purpose**: Groups multiple cache operations.

**Example**:
```java
@Caching(
    evict = {
        @CacheEvict(value = "users", key = "#id"),
        @CacheEvict(value = "userList", allEntries = true)
    }
)
public void deleteUser(Long id) {
    userRepository.deleteById(id);
}
```

### @CacheConfig

**Purpose**: Specifies default cache settings at the class level.

**Example**:
```java
@Service
@CacheConfig(cacheNames = "users")
public class UserService {
    @Cacheable
    public User getUser(Long id) {
        return userRepository.findById(id).orElse(null);
    }
}
```

## Scheduling Annotations

### @Scheduled

**Purpose**: Marks a method to be scheduled.

**Example**:
```java
@Component
public class ScheduledTasks {
    @Scheduled(fixedRate = 5000)
    public void executeTask() {
        // Execute every 5 seconds
    }
    
    @Scheduled(fixedDelay = 5000)
    public void executeAfterDelay() {
        // Execute 5 seconds after previous completion
    }
    
    @Scheduled(cron = "0 0 12 * * ?")
    public void executeDaily() {
        // Execute daily at 12 PM
    }
    
    @Scheduled(fixedRate = 5000, initialDelay = 10000)
    public void executeWithDelay() {
        // Start after 10 seconds, then every 5 seconds
    }
}
```

### @EnableScheduling

**Purpose**: Enables Spring's scheduled task execution capability.

**Example**:
```java
@SpringBootApplication
@EnableScheduling
public class Application {
    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}
```

## Async Annotations

### @Async

**Purpose**: Indicates that a method should be executed asynchronously.

**Example**:
```java
@Service
public class EmailService {
    @Async
    public void sendEmail(String to, String subject, String body) {
        // Async email sending
    }
    
    @Async("taskExecutor")
    public CompletableFuture<String> processData() {
        // Async processing with custom executor
        return CompletableFuture.completedFuture("Done");
    }
}
```

### @EnableAsync

**Purpose**: Enables Spring's asynchronous method execution capability.

**Example**:
```java
@SpringBootApplication
@EnableAsync
public class Application {
    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}
```

## AOP Annotations

### @Aspect

**Purpose**: Indicates that a class is an aspect.

**Example**:
```java
@Aspect
@Component
public class LoggingAspect {
    @Before("execution(* com.example.service.*.*(..))")
    public void logBefore(JoinPoint joinPoint) {
        // Logging logic
    }
}
```

### @Before

**Purpose**: Before advice that runs before a join point.

**Example**:
```java
@Aspect
@Component
public class LoggingAspect {
    @Before("execution(* com.example.service.*.*(..))")
    public void logBefore(JoinPoint joinPoint) {
        System.out.println("Before: " + joinPoint.getSignature().getName());
    }
}
```

### @After

**Purpose**: After advice that runs after a join point completes.

**Example**:
```java
@After("execution(* com.example.service.*.*(..))")
public void logAfter(JoinPoint joinPoint) {
    System.out.println("After: " + joinPoint.getSignature().getName());
}
```

### @AfterReturning

**Purpose**: After returning advice that runs after a method returns successfully.

**Example**:
```java
@AfterReturning(pointcut = "execution(* com.example.service.*.*(..))", returning = "result")
public void logAfterReturning(JoinPoint joinPoint, Object result) {
    System.out.println("Returned: " + result);
}
```

### @AfterThrowing

**Purpose**: After throwing advice that runs when a method throws an exception.

**Example**:
```java
@AfterThrowing(pointcut = "execution(* com.example.service.*.*(..))", throwing = "error")
public void logAfterThrowing(JoinPoint joinPoint, Throwable error) {
    System.out.println("Exception: " + error.getMessage());
}
```

### @Around

**Purpose**: Around advice that runs around a join point.

**Example**:
```java
@Around("execution(* com.example.service.*.*(..))")
public Object logAround(ProceedingJoinPoint joinPoint) throws Throwable {
    long start = System.currentTimeMillis();
    Object result = joinPoint.proceed();
    long duration = System.currentTimeMillis() - start;
    System.out.println("Duration: " + duration + "ms");
    return result;
}
```

### @Pointcut

**Purpose**: Defines a pointcut expression.

**Example**:
```java
@Aspect
@Component
public class LoggingAspect {
    @Pointcut("execution(* com.example.service.*.*(..))")
    public void serviceMethods() {}
    
    @Before("serviceMethods()")
    public void logBefore(JoinPoint joinPoint) {
        // Logging logic
    }
}
```

## Security Annotations

### @PreAuthorize

**Purpose**: Allows method-level security using SpEL expressions.

**Example**:
```java
@Service
@EnableMethodSecurity
public class UserService {
    @PreAuthorize("hasRole('ADMIN')")
    public void deleteUser(Long id) {
        userRepository.deleteById(id);
    }
    
    @PreAuthorize("hasRole('USER') or hasRole('ADMIN')")
    public User getUser(Long id) {
        return userRepository.findById(id).orElse(null);
    }
}
```

### @Secured

**Purpose**: Declares which security roles are allowed to invoke a method.

**Example**:
```java
@Service
@EnableGlobalMethodSecurity(securedEnabled = true)
public class UserService {
    @Secured("ROLE_ADMIN")
    public void deleteUser(Long id) {
        userRepository.deleteById(id);
    }
}
```

### @RolesAllowed

**Purpose**: Specifies the list of security roles permitted to access method(s).

**Example**:
```java
@Service
@RolesAllowed({"ADMIN", "USER"})
public class UserService {
    public User getUser(Long id) {
        return userRepository.findById(id).orElse(null);
    }
}
```

## Configuration Annotations

### @ConfigurationProperties

**Purpose**: Binds and validates external configuration properties.

**Example**:
```java
@ConfigurationProperties(prefix = "app")
public class AppProperties {
    private String name;
    private String version;
    private Database database;
    
    // Getters and setters
    
    public static class Database {
        private String url;
        private String username;
        // Getters and setters
    }
}
```

### @EnableConfigurationProperties

**Purpose**: Enables @ConfigurationProperties annotated beans.

**Example**:
```java
@Configuration
@EnableConfigurationProperties(AppProperties.class)
public class AppConfig {
}
```

### @ConditionalOnProperty

**Purpose**: Conditionally creates a bean based on a property value.

**Example**:
```java
@Bean
@ConditionalOnProperty(name = "feature.email.enabled", havingValue = "true")
public EmailService emailService() {
    return new EmailService();
}
```

### @ConditionalOnClass

**Purpose**: Conditionally creates a bean when a class is present.

**Example**:
```java
@Configuration
@ConditionalOnClass(DataSource.class)
public class DatabaseConfig {
    @Bean
    public DataSource dataSource() {
        return new HikariDataSource();
    }
}
```

### @ConditionalOnMissingBean

**Purpose**: Conditionally creates a bean when no bean of the specified type exists.

**Example**:
```java
@Bean
@ConditionalOnMissingBean
public UserService defaultUserService() {
    return new DefaultUserService();
}
```

### @Profile

**Purpose**: Indicates that a component is eligible for registration when specified profiles are active.

**Example**:
```java
@Configuration
@Profile("dev")
public class DevConfig {
    @Bean
    public DataSource devDataSource() {
        return new EmbeddedDatabaseBuilder()
            .setType(EmbeddedDatabaseType.H2)
            .build();
    }
}
```

## Testing Annotations

### @SpringBootTest

**Purpose**: Creates an application context for integration tests.

**Example**:
```java
@SpringBootTest
class UserServiceIntegrationTest {
    @Autowired
    private UserService userService;
    
    @Test
    void testCreateUser() {
        User user = new User("John", "john@example.com");
        User created = userService.createUser(user);
        assertNotNull(created.getId());
    }
}
```

### @WebMvcTest

**Purpose**: Slices the application context to only web layer components.

**Example**:
```java
@WebMvcTest(UserController.class)
class UserControllerTest {
    @Autowired
    private MockMvc mockMvc;
    
    @MockBean
    private UserService userService;
    
    @Test
    void testGetUser() throws Exception {
        when(userService.getUserById(1L)).thenReturn(new User());
        mockMvc.perform(get("/api/users/1"))
            .andExpect(status().isOk());
    }
}
```

### @DataJpaTest

**Purpose**: Slices the application context to only JPA components.

**Example**:
```java
@DataJpaTest
class UserRepositoryTest {
    @Autowired
    private TestEntityManager entityManager;
    
    @Autowired
    private UserRepository userRepository;
    
    @Test
    void testFindByName() {
        User user = new User("John", "john@example.com");
        entityManager.persist(user);
        Optional<User> found = userRepository.findByName("John");
        assertTrue(found.isPresent());
    }
}
```

### @MockBean

**Purpose**: Adds a mock bean to the Spring application context.

**Example**:
```java
@SpringBootTest
class UserServiceTest {
    @MockBean
    private UserRepository userRepository;
    
    @Autowired
    private UserService userService;
    
    @Test
    void testGetUser() {
        when(userRepository.findById(1L)).thenReturn(Optional.of(new User()));
        User user = userService.getUserById(1L);
        assertNotNull(user);
    }
}
```

### @SpyBean

**Purpose**: Adds a spy bean to the Spring application context.

**Example**:
```java
@SpringBootTest
class UserServiceTest {
    @SpyBean
    private UserService userService;
    
    @Test
    void testGetUser() {
        doReturn(new User()).when(userService).getUserById(1L);
        User user = userService.getUserById(1L);
        assertNotNull(user);
    }
}
```

## Actuator Annotations

### @Endpoint

**Purpose**: Marks a class as an actuator endpoint.

**Example**:
```java
@Component
@Endpoint(id = "custom")
public class CustomEndpoint {
    @ReadOperation
    public Map<String, Object> customInfo() {
        return Map.of("status", "UP");
    }
}
```

### @ReadOperation

**Purpose**: Marks a method as a read operation in an actuator endpoint.

**Example**:
```java
@ReadOperation
public Map<String, Object> getInfo() {
    return Map.of("status", "UP");
}
```

### @WriteOperation

**Purpose**: Marks a method as a write operation in an actuator endpoint.

**Example**:
```java
@WriteOperation
public void performAction(@Selector String action) {
    // Perform action
}
```

## Event Annotations

### @EventListener

**Purpose**: Marks a method as an event listener.

**Example**:
```java
@Component
public class UserEventListener {
    @EventListener
    public void handleUserCreated(UserCreatedEvent event) {
        System.out.println("User created: " + event.getUser().getName());
    }
    
    @Async
    @EventListener
    public void handleUserCreatedAsync(UserCreatedEvent event) {
        // Async processing
    }
}
```

### @TransactionalEventListener

**Purpose**: Marks a method as a transactional event listener.

**Example**:
```java
@Component
public class UserEventListener {
    @TransactionalEventListener(phase = TransactionPhase.AFTER_COMMIT)
    public void handleUserCreatedAfterCommit(UserCreatedEvent event) {
        emailService.sendWelcomeEmail(event.getUser());
    }
}
```

## Command Line Runner Annotations

### @Order

**Purpose**: Defines the sort order for an annotated component.

**Example**:
```java
@Component
@Order(1)
public class FirstRunner implements CommandLineRunner {
    @Override
    public void run(String... args) {
        System.out.println("First runner");
    }
}

@Component
@Order(2)
public class SecondRunner implements CommandLineRunner {
    @Override
    public void run(String... args) {
        System.out.println("Second runner");
    }
}
```

## Summary

This guide covers the most commonly used Spring Boot annotations organized by category:
- Core annotations for application setup
- Dependency injection annotations
- Web and REST annotations
- Validation annotations
- JPA and database annotations
- Transaction and caching annotations
- Scheduling and async annotations
- AOP annotations
- Security annotations
- Configuration annotations
- Testing annotations
- Actuator and monitoring annotations
- Event handling annotations

Each annotation serves a specific purpose in building robust Spring Boot applications.


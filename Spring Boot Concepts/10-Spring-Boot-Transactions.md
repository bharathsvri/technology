# Spring Boot Transactions

## What are Transactions?

A transaction is a sequence of operations performed as a single unit of work. Transactions ensure data consistency by following ACID properties:
- **Atomicity**: All or nothing
- **Consistency**: Data remains valid
- **Isolation**: Concurrent transactions don't interfere
- **Durability**: Committed changes are permanent

## Transaction Management in Spring

Spring provides declarative transaction management using `@Transactional` annotation, which eliminates the need for programmatic transaction management.

## @Transactional Annotation

### Basic Usage

```java
@Service
@Transactional
public class UserService {
    
    public User createUser(User user) {
        // This method runs in a transaction
        return userRepository.save(user);
    }
}
```

### Method-Level Transaction

```java
@Service
public class UserService {
    
    @Transactional
    public User createUser(User user) {
        return userRepository.save(user);
    }
    
    public User getUser(Long id) {
        // This method doesn't run in a transaction
        return userRepository.findById(id).orElse(null);
    }
}
```

## Transaction Propagation

### Propagation Types

#### REQUIRED (Default)
Uses existing transaction or creates new one:

```java
@Transactional(propagation = Propagation.REQUIRED)
public void method1() {
    // Uses existing transaction or creates new
}
```

#### REQUIRES_NEW
Always creates a new transaction:

```java
@Transactional(propagation = Propagation.REQUIRES_NEW)
public void method2() {
    // Always creates new transaction
}
```

#### SUPPORTS
Uses transaction if exists, otherwise runs without:

```java
@Transactional(propagation = Propagation.SUPPORTS)
public void method3() {
    // Uses transaction if available
}
```

#### NOT_SUPPORTED
Suspends current transaction if exists:

```java
@Transactional(propagation = Propagation.NOT_SUPPORTED)
public void method4() {
    // Runs without transaction
}
```

#### MANDATORY
Requires existing transaction, throws exception if none:

```java
@Transactional(propagation = Propagation.MANDATORY)
public void method5() {
    // Requires existing transaction
}
```

#### NEVER
Throws exception if transaction exists:

```java
@Transactional(propagation = Propagation.NEVER)
public void method6() {
    // Must not run in transaction
}
```

#### NESTED
Creates nested transaction if transaction exists:

```java
@Transactional(propagation = Propagation.NESTED)
public void method7() {
    // Creates nested transaction
}
```

## Transaction Isolation

### Isolation Levels

#### READ_UNCOMMITTED
Lowest isolation, allows dirty reads:

```java
@Transactional(isolation = Isolation.READ_UNCOMMITTED)
public void method() {
    // ...
}
```

#### READ_COMMITTED
Prevents dirty reads, allows non-repeatable reads:

```java
@Transactional(isolation = Isolation.READ_COMMITTED)
public void method() {
    // ...
}
```

#### REPEATABLE_READ
Prevents dirty and non-repeatable reads:

```java
@Transactional(isolation = Isolation.REPEATABLE_READ)
public void method() {
    // ...
}
```

#### SERIALIZABLE
Highest isolation, prevents all concurrency issues:

```java
@Transactional(isolation = Isolation.SERIALIZABLE)
public void method() {
    // ...
}
```

## Transaction Rollback

### Default Rollback Behavior

By default, transactions rollback on `RuntimeException` and `Error`, but not on checked exceptions.

### Custom Rollback Rules

```java
@Transactional(rollbackFor = Exception.class)
public void method() {
    // Rolls back on any exception
}

@Transactional(noRollbackFor = IllegalArgumentException.class)
public void method() {
    // Doesn't rollback on IllegalArgumentException
}
```

## Transaction Timeout

### Setting Timeout

```java
@Transactional(timeout = 30)
public void method() {
    // Transaction times out after 30 seconds
}
```

## Read-Only Transactions

### Read-Only Mode

```java
@Transactional(readOnly = true)
public List<User> getAllUsers() {
    // Optimized for read operations
    return userRepository.findAll();
}
```

## Programmatic Transaction Management

### Using TransactionTemplate

```java
@Service
public class UserService {
    
    private final TransactionTemplate transactionTemplate;
    
    public UserService(PlatformTransactionManager transactionManager) {
        this.transactionTemplate = new TransactionTemplate(transactionManager);
    }
    
    public User createUser(User user) {
        return transactionTemplate.execute(status -> {
            return userRepository.save(user);
        });
    }
}
```

### Using PlatformTransactionManager

```java
@Service
public class UserService {
    
    @Autowired
    private PlatformTransactionManager transactionManager;
    
    public User createUser(User user) {
        TransactionStatus status = transactionManager.getTransaction(
            new DefaultTransactionDefinition()
        );
        try {
            User savedUser = userRepository.save(user);
            transactionManager.commit(status);
            return savedUser;
        } catch (Exception e) {
            transactionManager.rollback(status);
            throw e;
        }
    }
}
```

## Transaction Best Practices

### 1. Use @Transactional in Service Layer

```java
@Service
@Transactional
public class UserService {
    // Service methods are transactional
}
```

### 2. Avoid @Transactional on Private Methods

```java
// ❌ Bad
@Transactional
private void privateMethod() {
    // Transaction won't work
}

// ✅ Good
@Transactional
public void publicMethod() {
    // Transaction works
}
```

### 3. Use Read-Only for Queries

```java
@Transactional(readOnly = true)
public List<User> getAllUsers() {
    return userRepository.findAll();
}
```

### 4. Handle Exceptions Properly

```java
@Transactional
public void transferMoney(Long fromId, Long toId, BigDecimal amount) {
    try {
        accountService.debit(fromId, amount);
        accountService.credit(toId, amount);
    } catch (InsufficientFundsException e) {
        // Transaction will rollback automatically
        throw e;
    }
}
```

## Complete Example

### Service with Transactions

```java
@Service
@Transactional
public class OrderService {
    
    private final OrderRepository orderRepository;
    private final UserRepository userRepository;
    private final PaymentService paymentService;
    
    public OrderService(OrderRepository orderRepository,
                       UserRepository userRepository,
                       PaymentService paymentService) {
        this.orderRepository = orderRepository;
        this.userRepository = userRepository;
        this.paymentService = paymentService;
    }
    
    @Transactional(rollbackFor = Exception.class)
    public Order createOrder(Order order) {
        // Validate user
        User user = userRepository.findById(order.getUserId())
            .orElseThrow(() -> new ResourceNotFoundException("User not found"));
        
        // Process payment
        paymentService.processPayment(order.getPayment());
        
        // Create order
        Order savedOrder = orderRepository.save(order);
        
        // Update user
        user.addOrder(savedOrder);
        userRepository.save(user);
        
        return savedOrder;
    }
    
    @Transactional(readOnly = true)
    public Order getOrder(Long id) {
        return orderRepository.findById(id)
            .orElseThrow(() -> new ResourceNotFoundException("Order not found"));
    }
    
    @Transactional(propagation = Propagation.REQUIRES_NEW)
    public void logOrder(Order order) {
        // This runs in a separate transaction
        // Even if parent transaction fails, this will commit
        orderLogRepository.save(new OrderLog(order));
    }
}
```

## Distributed Transactions

### Using JTA (Java Transaction API)

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-jta-atomikos</artifactId>
</dependency>
```

```java
@Configuration
@EnableTransactionManagement
public class TransactionConfig {
    // JTA configuration
}
```

## Transaction Events

### Using @TransactionalEventListener

```java
@Component
public class OrderEventListener {
    
    @TransactionalEventListener(phase = TransactionPhase.AFTER_COMMIT)
    public void handleOrderCreated(OrderCreatedEvent event) {
        // This runs after transaction commits
        emailService.sendOrderConfirmation(event.getOrder());
    }
}
```

## Testing Transactions

### Using @Transactional in Tests

```java
@SpringBootTest
@Transactional
class UserServiceTest {
    
    @Autowired
    private UserService userService;
    
    @Test
    void testCreateUser() {
        User user = new User("John", "john@example.com");
        User savedUser = userService.createUser(user);
        assertNotNull(savedUser.getId());
    }
}
```

### Rolling Back Test Transactions

```java
@SpringBootTest
@Transactional
@Rollback
class UserServiceTest {
    // Transactions rollback after test
}
```

## Common Transaction Issues

### Issue 1: Transaction Not Working

**Cause**: Method is private or called from same class

**Solution**: Make method public and call from different bean

### Issue 2: Transaction Rollback Not Working

**Cause**: Catching and swallowing exceptions

**Solution**: Let exceptions propagate or use `rollbackFor`

### Issue 3: Nested Transaction Not Working

**Cause**: Using `REQUIRED` instead of `REQUIRES_NEW`

**Solution**: Use `REQUIRES_NEW` for nested transactions

## Summary

Spring Boot Transactions:
- Use `@Transactional` for declarative transaction management
- Support different propagation levels
- Support different isolation levels
- Handle rollback automatically
- Support read-only transactions
- Can be configured programmatically
- Essential for data consistency


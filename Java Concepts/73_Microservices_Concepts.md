# Microservices Concepts

## What are Microservices?
Microservices is an architectural approach where applications are built as a collection of small, independent services.

## Key Principles

### Service Independence
```java
// Each microservice is independent
@SpringBootApplication
public class UserServiceApplication {
    public static void main(String[] args) {
        SpringApplication.run(UserServiceApplication.class, args);
    }
}
```

### API Gateway Pattern
```java
@RestController
@RequestMapping("/api")
public class ApiGateway {
    
    @Autowired
    private UserServiceClient userService;
    
    @Autowired
    private OrderServiceClient orderService;
    
    @GetMapping("/user/{id}/orders")
    public ResponseEntity<?> getUserOrders(@PathVariable Long id) {
        User user = userService.getUser(id);
        List<Order> orders = orderService.getUserOrders(id);
        return ResponseEntity.ok(new UserOrdersResponse(user, orders));
    }
}
```

## Service Discovery

### Eureka Client
```java
@SpringBootApplication
@EnableEurekaClient
public class UserServiceApplication {
    public static void main(String[] args) {
        SpringApplication.run(UserServiceApplication.class, args);
    }
}
```

## Inter-Service Communication

### REST Client
```java
@Service
public class OrderServiceClient {
    
    @Autowired
    private RestTemplate restTemplate;
    
    public List<Order> getUserOrders(Long userId) {
        String url = "http://order-service/api/orders/user/" + userId;
        ResponseEntity<List<Order>> response = restTemplate.exchange(
            url, HttpMethod.GET, null, 
            new ParameterizedTypeReference<List<Order>>() {}
        );
        return response.getBody();
    }
}
```

### Feign Client
```java
@FeignClient(name = "order-service")
public interface OrderServiceClient {
    @GetMapping("/api/orders/user/{userId}")
    List<Order> getUserOrders(@PathVariable Long userId);
}
```

## Configuration Management

### Spring Cloud Config
```java
@SpringBootApplication
@EnableConfigServer
public class ConfigServerApplication {
    public static void main(String[] args) {
        SpringApplication.run(ConfigServerApplication.class, args);
    }
}
```

## Circuit Breaker

### Resilience4j
```java
@Service
public class UserService {
    
    @CircuitBreaker(name = "userService", fallbackMethod = "fallback")
    public User getUser(Long id) {
        // Call external service
        return userRepository.findById(id);
    }
    
    public User fallback(Long id, Exception e) {
        return new User(id, "Default User");
    }
}
```

## Best Practices
1. Design services around business capabilities
2. Keep services small and focused
3. Use API Gateway for routing
4. Implement service discovery
5. Handle failures gracefully (Circuit Breaker)
6. Use distributed tracing
7. Implement health checks
8. Use containerization (Docker)
9. Implement proper logging
10. Secure inter-service communication


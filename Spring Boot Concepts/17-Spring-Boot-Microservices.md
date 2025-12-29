# Spring Boot Microservices

## What are Microservices?

Microservices is an architectural approach where applications are built as a collection of small, independent services that communicate over well-defined APIs.

## Microservices with Spring Boot

Spring Boot is ideal for building microservices due to:
- Embedded servers
- Auto-configuration
- Production-ready features
- Easy deployment

## Service Discovery

### Eureka Server

#### Add Dependency

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-eureka-server</artifactId>
</dependency>
```

#### Enable Eureka Server

```java
@SpringBootApplication
@EnableEurekaServer
public class EurekaServerApplication {
    public static void main(String[] args) {
        SpringApplication.run(EurekaServerApplication.class, args);
    }
}
```

#### Configuration

```properties
server.port=8761
eureka.client.register-with-eureka=false
eureka.client.fetch-registry=false
```

### Eureka Client

#### Add Dependency

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
</dependency>
```

#### Enable Eureka Client

```java
@SpringBootApplication
@EnableEurekaClient
public class UserServiceApplication {
    public static void main(String[] args) {
        SpringApplication.run(UserServiceApplication.class, args);
    }
}
```

#### Configuration

```properties
spring.application.name=user-service
eureka.client.service-url.defaultZone=http://localhost:8761/eureka/
```

## API Gateway

### Spring Cloud Gateway

#### Add Dependency

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-gateway</artifactId>
</dependency>
```

#### Gateway Configuration

```yaml
spring:
  cloud:
    gateway:
      routes:
        - id: user-service
          uri: lb://user-service
          predicates:
            - Path=/api/users/**
          filters:
            - StripPrefix=1
        - id: order-service
          uri: lb://order-service
          predicates:
            - Path=/api/orders/**
          filters:
            - StripPrefix=1
```

## Service Communication

### RestTemplate

```java
@Service
public class UserService {
    
    @Autowired
    private RestTemplate restTemplate;
    
    public Order getOrder(Long orderId) {
        return restTemplate.getForObject(
            "http://order-service/api/orders/" + orderId,
            Order.class
        );
    }
}

@Configuration
public class RestTemplateConfig {
    
    @Bean
    @LoadBalanced
    public RestTemplate restTemplate() {
        return new RestTemplate();
    }
}
```

### WebClient

```java
@Service
public class UserService {
    
    private final WebClient webClient;
    
    public UserService(WebClient.Builder webClientBuilder) {
        this.webClient = webClientBuilder
            .baseUrl("http://order-service")
            .build();
    }
    
    public Mono<Order> getOrder(Long orderId) {
        return webClient.get()
            .uri("/api/orders/{id}", orderId)
            .retrieve()
            .bodyToMono(Order.class);
    }
}
```

### Feign Client

#### Add Dependency

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-openfeign</artifactId>
</dependency>
```

#### Enable Feign Clients

```java
@SpringBootApplication
@EnableFeignClients
public class Application {
    // ...
}
```

#### Feign Client Interface

```java
@FeignClient(name = "order-service")
public interface OrderClient {
    
    @GetMapping("/api/orders/{id}")
    Order getOrder(@PathVariable Long id);
    
    @PostMapping("/api/orders")
    Order createOrder(@RequestBody Order order);
}
```

## Configuration Management

### Spring Cloud Config Server

#### Add Dependency

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-config-server</artifactId>
</dependency>
```

#### Enable Config Server

```java
@SpringBootApplication
@EnableConfigServer
public class ConfigServerApplication {
    public static void main(String[] args) {
        SpringApplication.run(ConfigServerApplication.class, args);
    }
}
```

#### Configuration

```properties
spring.cloud.config.server.git.uri=file:///path/to/config-repo
```

### Config Client

#### Add Dependency

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-config</artifactId>
</dependency>
```

#### Configuration

```properties
spring.application.name=user-service
spring.cloud.config.uri=http://localhost:8888
```

## Circuit Breaker

### Resilience4j

#### Add Dependency

```xml
<dependency>
    <groupId>io.github.resilience4j</groupId>
    <artifactId>resilience4j-spring-boot2</artifactId>
</dependency>
```

#### Using Circuit Breaker

```java
@Service
public class UserService {
    
    @CircuitBreaker(name = "orderService", fallbackMethod = "getOrderFallback")
    public Order getOrder(Long orderId) {
        return orderClient.getOrder(orderId);
    }
    
    public Order getOrderFallback(Long orderId, Exception e) {
        return new Order(); // Fallback response
    }
}
```

## Distributed Tracing

### Sleuth and Zipkin

#### Add Dependency

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-sleuth</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-sleuth-zipkin</artifactId>
</dependency>
```

#### Configuration

```properties
spring.zipkin.base-url=http://localhost:9411
spring.sleuth.sampler.probability=1.0
```

## Best Practices

1. **Service Independence**: Keep services independent
2. **API Versioning**: Version your APIs
3. **Centralized Configuration**: Use config server
4. **Service Discovery**: Use service discovery
5. **Circuit Breakers**: Implement circuit breakers
6. **Distributed Tracing**: Use tracing for debugging
7. **API Gateway**: Use gateway for routing
8. **Health Checks**: Implement health checks

## Summary

Spring Boot Microservices:
- Easy to build with Spring Boot
- Supports service discovery
- Provides API gateway
- Enables distributed tracing
- Essential for microservices architecture


# Spring Boot WebFlux

## What is WebFlux?

Spring WebFlux is a reactive web framework built on Project Reactor. It provides non-blocking, asynchronous programming for building reactive applications.

## Setting Up WebFlux

### Add Dependency

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-webflux</artifactId>
</dependency>
```

## Reactive Controllers

### REST Controller

```java
@RestController
@RequestMapping("/api/users")
public class UserController {
    
    private final UserService userService;
    
    @GetMapping
    public Flux<User> getAllUsers() {
        return userService.getAllUsers();
    }
    
    @GetMapping("/{id}")
    public Mono<User> getUserById(@PathVariable String id) {
        return userService.getUserById(id);
    }
    
    @PostMapping
    public Mono<ResponseEntity<User>> createUser(@RequestBody Mono<User> userMono) {
        return userService.createUser(userMono)
            .map(user -> ResponseEntity.status(HttpStatus.CREATED).body(user));
    }
    
    @PutMapping("/{id}")
    public Mono<ResponseEntity<User>> updateUser(
            @PathVariable String id,
            @RequestBody Mono<User> userMono) {
        return userService.updateUser(id, userMono)
            .map(user -> ResponseEntity.ok(user));
    }
    
    @DeleteMapping("/{id}")
    public Mono<ResponseEntity<Void>> deleteUser(@PathVariable String id) {
        return userService.deleteUser(id)
            .then(Mono.just(ResponseEntity.noContent().build()));
    }
}
```

## Reactive Service

### Service Implementation

```java
@Service
public class UserService {
    
    private final UserRepository userRepository;
    
    public Flux<User> getAllUsers() {
        return userRepository.findAll();
    }
    
    public Mono<User> getUserById(String id) {
        return userRepository.findById(id)
            .switchIfEmpty(Mono.error(new ResourceNotFoundException("User not found")));
    }
    
    public Mono<User> createUser(Mono<User> userMono) {
        return userMono
            .flatMap(userRepository::save)
            .doOnNext(user -> System.out.println("User created: " + user.getId()));
    }
    
    public Mono<User> updateUser(String id, Mono<User> userMono) {
        return userMono
            .flatMap(user -> {
                user.setId(id);
                return userRepository.save(user);
            });
    }
    
    public Mono<Void> deleteUser(String id) {
        return userRepository.deleteById(id);
    }
}
```

## Reactive Repository

### MongoDB Reactive Repository

```java
public interface UserRepository extends ReactiveMongoRepository<User, String> {
    Flux<User> findByName(String name);
    Mono<User> findByEmail(String email);
}
```

## WebClient

### Using WebClient

```java
@Service
public class ExternalService {
    
    private final WebClient webClient;
    
    public ExternalService(WebClient.Builder webClientBuilder) {
        this.webClient = webClientBuilder
            .baseUrl("https://api.example.com")
            .build();
    }
    
    public Mono<User> getUserFromExternalService(String id) {
        return webClient.get()
            .uri("/users/{id}", id)
            .retrieve()
            .bodyToMono(User.class)
            .timeout(Duration.ofSeconds(5))
            .retry(3);
    }
    
    public Flux<User> getAllUsersFromExternalService() {
        return webClient.get()
            .uri("/users")
            .retrieve()
            .bodyToFlux(User.class);
    }
}
```

## Router Functions

### Functional Endpoints

```java
@Configuration
public class RouterConfig {
    
    @Bean
    public RouterFunction<ServerResponse> routes(UserHandler userHandler) {
        return RouterFunctions.route()
            .GET("/api/users", userHandler::getAllUsers)
            .GET("/api/users/{id}", userHandler::getUserById)
            .POST("/api/users", userHandler::createUser)
            .PUT("/api/users/{id}", userHandler::updateUser)
            .DELETE("/api/users/{id}", userHandler::deleteUser)
            .build();
    }
}
```

### Handler

```java
@Component
public class UserHandler {
    
    private final UserService userService;
    
    public Mono<ServerResponse> getAllUsers(ServerRequest request) {
        return ServerResponse.ok()
            .contentType(MediaType.APPLICATION_JSON)
            .body(userService.getAllUsers(), User.class);
    }
    
    public Mono<ServerResponse> getUserById(ServerRequest request) {
        String id = request.pathVariable("id");
        return userService.getUserById(id)
            .flatMap(user -> ServerResponse.ok()
                .contentType(MediaType.APPLICATION_JSON)
                .bodyValue(user))
            .switchIfEmpty(ServerResponse.notFound().build());
    }
    
    public Mono<ServerResponse> createUser(ServerRequest request) {
        return request.bodyToMono(User.class)
            .flatMap(userService::createUser)
            .flatMap(user -> ServerResponse.status(HttpStatus.CREATED)
                .contentType(MediaType.APPLICATION_JSON)
                .bodyValue(user));
    }
}
```

## Error Handling

### Global Error Handler

```java
@Component
public class GlobalErrorHandler implements WebExceptionHandler {
    
    @Override
    public Mono<Void> handle(ServerWebExchange exchange, Throwable ex) {
        ServerHttpResponse response = exchange.getResponse();
        
        if (ex instanceof ResourceNotFoundException) {
            response.setStatusCode(HttpStatus.NOT_FOUND);
        } else if (ex instanceof ValidationException) {
            response.setStatusCode(HttpStatus.BAD_REQUEST);
        } else {
            response.setStatusCode(HttpStatus.INTERNAL_SERVER_ERROR);
        }
        
        return response.setComplete();
    }
}
```

## Server-Sent Events

### SSE Endpoint

```java
@GetMapping(value = "/events", produces = MediaType.TEXT_EVENT_STREAM_VALUE)
public Flux<ServerSentEvent<String>> streamEvents() {
    return Flux.interval(Duration.ofSeconds(1))
        .map(sequence -> ServerSentEvent.<String>builder()
            .id(String.valueOf(sequence))
            .event("user-event")
            .data("Event " + sequence)
            .build());
}
```

## Best Practices

1. **Use Reactive Types**: Use Mono and Flux consistently
2. **Avoid Blocking**: Never block in reactive code
3. **Handle Errors**: Use error handling operators
4. **Use Backpressure**: Handle backpressure properly
5. **Test Reactively**: Use StepVerifier for testing

## Summary

Spring Boot WebFlux:
- Non-blocking reactive programming
- High performance and scalability
- Supports reactive streams
- Essential for reactive applications


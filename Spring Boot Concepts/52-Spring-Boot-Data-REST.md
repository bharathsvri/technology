# Spring Boot Data REST

## What is Spring Data REST?

Spring Data REST automatically exposes JPA repositories as RESTful endpoints, eliminating the need to write controllers for basic CRUD operations.

## Setting Up Data REST

### Add Dependency

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-rest</artifactId>
</dependency>
```

### Configuration

```properties
# Data REST Configuration
spring.data.rest.base-path=/api
spring.data.rest.default-page-size=20
spring.data.rest.max-page-size=100
spring.data.rest.return-body-on-create=true
spring.data.rest.return-body-on-update=true
```

## Basic Repository

### Entity

```java
@Entity
@Table(name = "users")
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    private String name;
    private String email;
    private Integer age;
    
    // Constructors, getters, setters
}
```

### Repository

```java
@RepositoryRestResource(collectionResourceRel = "users", path = "users")
public interface UserRepository extends JpaRepository<User, Long> {
    List<User> findByName(String name);
    List<User> findByEmail(String email);
}
```

### Automatic Endpoints

- `GET /api/users` - Get all users
- `GET /api/users/{id}` - Get user by ID
- `POST /api/users` - Create user
- `PUT /api/users/{id}` - Update user
- `PATCH /api/users/{id}` - Partial update
- `DELETE /api/users/{id}` - Delete user
- `GET /api/users/search` - Search users

## Custom Queries

### Exposing Custom Queries

```java
@RepositoryRestResource(collectionResourceRel = "users", path = "users")
public interface UserRepository extends JpaRepository<User, Long> {
    
    @RestResource(path = "by-name", rel = "findByName")
    List<User> findByName(@Param("name") String name);
    
    @RestResource(path = "by-email", rel = "findByEmail")
    User findByEmail(@Param("email") String email);
    
    @RestResource(exported = false)
    List<User> findByAge(Integer age); // Not exposed
}
```

### Accessing Custom Queries

```bash
GET /api/users/search/by-name?name=John
GET /api/users/search/by-email?email=john@example.com
```

## Projections

### Creating Projection

```java
@Projection(name = "summary", types = {User.class})
public interface UserSummary {
    String getName();
    String getEmail();
}
```

### Using Projection

```bash
GET /api/users/1?projection=summary
GET /api/users?projection=summary
```

## Customizing Exports

### Repository Configuration

```java
@RepositoryRestResource(
    collectionResourceRel = "users",
    path = "users",
    excerptProjection = UserSummary.class
)
public interface UserRepository extends JpaRepository<User, Long> {
}
```

## Security

### Securing Endpoints

```java
@Configuration
public class DataRestSecurityConfig {
    
    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
        http
            .authorizeHttpRequests(auth -> auth
                .requestMatchers("/api/users/**").hasRole("USER")
                .requestMatchers("/api/admin/**").hasRole("ADMIN")
                .anyRequest().authenticated()
            );
        return http.build();
    }
}
```

## Event Handlers

### Repository Event Handlers

```java
@Component
@RepositoryEventHandler(User.class)
public class UserEventHandler {
    
    @HandleBeforeCreate
    public void handleUserBeforeCreate(User user) {
        System.out.println("Before creating user: " + user.getName());
    }
    
    @HandleAfterCreate
    public void handleUserAfterCreate(User user) {
        System.out.println("After creating user: " + user.getName());
    }
    
    @HandleBeforeSave
    public void handleUserBeforeSave(User user) {
        System.out.println("Before saving user: " + user.getName());
    }
    
    @HandleAfterSave
    public void handleUserAfterSave(User user) {
        System.out.println("After saving user: " + user.getName());
    }
    
    @HandleBeforeDelete
    public void handleUserBeforeDelete(User user) {
        System.out.println("Before deleting user: " + user.getName());
    }
    
    @HandleAfterDelete
    public void handleUserAfterDelete(User user) {
        System.out.println("After deleting user: " + user.getName());
    }
}
```

## Best Practices

1. **Use Projections**: Use projections to limit exposed data
2. **Secure Endpoints**: Secure REST endpoints properly
3. **Custom Queries**: Expose custom queries when needed
4. **Hide Sensitive Data**: Don't expose sensitive repositories
5. **Document Endpoints**: Document custom endpoints

## Summary

Spring Boot Data REST:
- Automatic REST endpoints
- Reduces boilerplate code
- Supports projections and custom queries
- Essential for rapid API development


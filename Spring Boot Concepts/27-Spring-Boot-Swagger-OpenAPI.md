# Spring Boot Swagger/OpenAPI

## What is OpenAPI?

OpenAPI (formerly Swagger) is a specification for describing REST APIs. It provides a standard way to document APIs that both humans and machines can read.

## Setting Up Swagger/OpenAPI

### Add Dependency

```xml
<dependency>
    <groupId>org.springdoc</groupId>
    <artifactId>springdoc-openapi-starter-webmvc-ui</artifactId>
    <version>2.2.0</version>
</dependency>
```

### Configuration

```properties
springdoc.api-docs.path=/api-docs
springdoc.swagger-ui.path=/swagger-ui.html
springdoc.swagger-ui.operationsSorter=method
springdoc.swagger-ui.tagsSorter=alpha
```

## Basic Configuration

### OpenAPI Configuration

```java
@Configuration
public class OpenApiConfig {
    
    @Bean
    public OpenAPI customOpenAPI() {
        return new OpenAPI()
            .info(new Info()
                .title("User Management API")
                .version("1.0.0")
                .description("API for managing users")
                .contact(new Contact()
                    .name("API Support")
                    .email("support@example.com"))
                .license(new License()
                    .name("Apache 2.0")
                    .url("http://www.apache.org/licenses/LICENSE-2.0.html")))
            .servers(Arrays.asList(
                new Server().url("http://localhost:8080").description("Development Server"),
                new Server().url("https://api.example.com").description("Production Server")
            ));
    }
}
```

## Annotating Controllers

### Basic Annotations

```java
@RestController
@RequestMapping("/api/users")
@Tag(name = "Users", description = "User management APIs")
public class UserController {
    
    @Operation(summary = "Get all users", description = "Retrieve all users from the system")
    @ApiResponses(value = {
        @ApiResponse(responseCode = "200", description = "Successfully retrieved users"),
        @ApiResponse(responseCode = "404", description = "Users not found")
    })
    @GetMapping
    public ResponseEntity<List<User>> getAllUsers() {
        return ResponseEntity.ok(userService.getAllUsers());
    }
    
    @Operation(summary = "Get user by ID", description = "Retrieve a specific user by their ID")
    @GetMapping("/{id}")
    public ResponseEntity<User> getUserById(
            @Parameter(description = "User ID", required = true) @PathVariable Long id) {
        return ResponseEntity.ok(userService.getUserById(id));
    }
    
    @Operation(summary = "Create user", description = "Create a new user in the system")
    @PostMapping
    public ResponseEntity<User> createUser(
            @io.swagger.v3.oas.annotations.parameters.RequestBody(
                description = "User object to be created",
                required = true,
                content = @Content(schema = @Schema(implementation = User.class))
            ) @RequestBody User user) {
        return ResponseEntity.ok(userService.createUser(user));
    }
}
```

## Documenting Models

### Entity Documentation

```java
@Schema(description = "User entity representing a user in the system")
public class User {
    
    @Schema(description = "User ID", example = "1", accessMode = Schema.AccessMode.READ_ONLY)
    private Long id;
    
    @Schema(description = "User's full name", example = "John Doe", required = true)
    @NotBlank
    private String name;
    
    @Schema(description = "User's email address", example = "john@example.com", required = true)
    @Email
    private String email;
    
    @Schema(description = "User's age", example = "30", minimum = "18", maximum = "100")
    @Min(18)
    @Max(100)
    private Integer age;
    
    // Constructors, getters, setters
}
```

## Security Documentation

### JWT Security

```java
@Configuration
public class OpenApiConfig {
    
    @Bean
    public OpenAPI customOpenAPI() {
        return new OpenAPI()
            .info(new Info().title("API").version("1.0.0"))
            .addSecurityItem(new SecurityRequirement().addList("bearerAuth"))
            .components(new Components()
                .addSecuritySchemes("bearerAuth", new SecurityScheme()
                    .type(SecurityScheme.Type.HTTP)
                    .scheme("bearer")
                    .bearerFormat("JWT")
                    .in(SecurityScheme.In.HEADER)
                    .name("Authorization")));
    }
}
```

### Using Security in Endpoints

```java
@Operation(summary = "Get user profile", security = @SecurityRequirement(name = "bearerAuth"))
@GetMapping("/profile")
public ResponseEntity<User> getProfile() {
    return ResponseEntity.ok(userService.getCurrentUser());
}
```

## Grouping APIs

### Multiple API Groups

```java
@Configuration
public class OpenApiConfig {
    
    @Bean
    public GroupedOpenApi publicApi() {
        return GroupedOpenApi.builder()
            .group("public")
            .pathsToMatch("/api/public/**")
            .build();
    }
    
    @Bean
    public GroupedOpenApi adminApi() {
        return GroupedOpenApi.builder()
            .group("admin")
            .pathsToMatch("/api/admin/**")
            .build();
    }
}
```

## Customizing Swagger UI

### Custom Configuration

```properties
springdoc.swagger-ui.enabled=true
springdoc.swagger-ui.path=/swagger-ui.html
springdoc.swagger-ui.operationsSorter=method
springdoc.swagger-ui.tagsSorter=alpha
springdoc.swagger-ui.doc-expansion=none
springdoc.swagger-ui.filter=true
springdoc.swagger-ui.tryItOutEnabled=true
```

## Accessing Documentation

### Swagger UI
- URL: `http://localhost:8080/swagger-ui.html`

### OpenAPI JSON
- URL: `http://localhost:8080/api-docs`

### YAML Format
- URL: `http://localhost:8080/api-docs.yaml`

## Best Practices

1. **Document All Endpoints**: Document all API endpoints
2. **Use Descriptive Descriptions**: Provide clear descriptions
3. **Include Examples**: Add examples for requests and responses
4. **Document Errors**: Document possible error responses
5. **Keep Updated**: Keep documentation in sync with code

## Summary

Spring Boot Swagger/OpenAPI:
- Automatic API documentation
- Interactive API testing
- Standard OpenAPI specification
- Essential for API development


# Spring Boot REST APIs

## What is REST?

REST (Representational State Transfer) is an architectural style for designing networked applications. REST APIs use HTTP methods to perform operations on resources.

## REST Principles

1. **Stateless**: Each request contains all information needed
2. **Client-Server**: Separation of concerns
3. **Uniform Interface**: Consistent way of interacting
4. **Resource-Based**: Everything is a resource
5. **HTTP Methods**: GET, POST, PUT, DELETE, PATCH

## Creating REST Controller

### Basic REST Controller

```java
@RestController
@RequestMapping("/api/users")
public class UserController {
    
    @GetMapping
    public List<User> getAllUsers() {
        return userService.getAllUsers();
    }
    
    @GetMapping("/{id}")
    public User getUserById(@PathVariable Long id) {
        return userService.getUserById(id);
    }
    
    @PostMapping
    public User createUser(@RequestBody User user) {
        return userService.createUser(user);
    }
    
    @PutMapping("/{id}")
    public User updateUser(@PathVariable Long id, @RequestBody User user) {
        return userService.updateUser(id, user);
    }
    
    @DeleteMapping("/{id}")
    public void deleteUser(@PathVariable Long id) {
        userService.deleteUser(id);
    }
}
```

## HTTP Methods

### GET - Retrieve Resources

```java
@GetMapping("/users")
public List<User> getUsers() {
    return userService.getAllUsers();
}

@GetMapping("/users/{id}")
public ResponseEntity<User> getUser(@PathVariable Long id) {
    User user = userService.getUserById(id);
    if (user != null) {
        return ResponseEntity.ok(user);
    }
    return ResponseEntity.notFound().build();
}
```

### POST - Create Resources

```java
@PostMapping("/users")
public ResponseEntity<User> createUser(@RequestBody User user) {
    User createdUser = userService.createUser(user);
    return ResponseEntity.status(HttpStatus.CREATED)
            .body(createdUser);
}
```

### PUT - Update Resources (Full Update)

```java
@PutMapping("/users/{id}")
public ResponseEntity<User> updateUser(
        @PathVariable Long id,
        @RequestBody User user) {
    User updatedUser = userService.updateUser(id, user);
    return ResponseEntity.ok(updatedUser);
}
```

### PATCH - Partial Update

```java
@PatchMapping("/users/{id}")
public ResponseEntity<User> partialUpdateUser(
        @PathVariable Long id,
        @RequestBody Map<String, Object> updates) {
    User updatedUser = userService.partialUpdateUser(id, updates);
    return ResponseEntity.ok(updatedUser);
}
```

### DELETE - Delete Resources

```java
@DeleteMapping("/users/{id}")
public ResponseEntity<Void> deleteUser(@PathVariable Long id) {
    userService.deleteUser(id);
    return ResponseEntity.noContent().build();
}
```

## Request Mapping Annotations

### @RequestMapping

```java
@RequestMapping(value = "/api/users", method = RequestMethod.GET)
public List<User> getUsers() {
    // ...
}
```

### Shortcut Annotations

```java
@GetMapping("/users")      // @RequestMapping(method = RequestMethod.GET)
@PostMapping("/users")     // @RequestMapping(method = RequestMethod.POST)
@PutMapping("/users/{id}") // @RequestMapping(method = RequestMethod.PUT)
@DeleteMapping("/users/{id}") // @RequestMapping(method = RequestMethod.DELETE)
@PatchMapping("/users/{id}") // @RequestMapping(method = RequestMethod.PATCH)
```

## Path Variables

### Single Path Variable

```java
@GetMapping("/users/{id}")
public User getUser(@PathVariable Long id) {
    return userService.getUserById(id);
}
```

### Multiple Path Variables

```java
@GetMapping("/users/{userId}/posts/{postId}")
public Post getPost(
        @PathVariable Long userId,
        @PathVariable Long postId) {
    return postService.getPost(userId, postId);
}
```

### Named Path Variables

```java
@GetMapping("/users/{userId}/posts/{postId}")
public Post getPost(
        @PathVariable("userId") Long userId,
        @PathVariable("postId") Long postId) {
    // ...
}
```

## Request Parameters

### Query Parameters

```java
@GetMapping("/users")
public List<User> getUsers(
        @RequestParam(defaultValue = "0") int page,
        @RequestParam(defaultValue = "10") int size,
        @RequestParam(required = false) String name) {
    return userService.getUsers(page, size, name);
}
```

### Optional Parameters

```java
@GetMapping("/search")
public List<User> searchUsers(
        @RequestParam(required = false) String name,
        @RequestParam(required = false) String email) {
    return userService.searchUsers(name, email);
}
```

### Multiple Values

```java
@GetMapping("/users")
public List<User> getUsers(@RequestParam List<Long> ids) {
    return userService.getUsersByIds(ids);
}
```

## Request Body

### JSON Request Body

```java
@PostMapping("/users")
public User createUser(@RequestBody User user) {
    return userService.createUser(user);
}
```

### Validating Request Body

```java
@PostMapping("/users")
public ResponseEntity<User> createUser(@Valid @RequestBody User user) {
    User createdUser = userService.createUser(user);
    return ResponseEntity.status(HttpStatus.CREATED).body(createdUser);
}
```

## Response Entity

### Custom Response

```java
@GetMapping("/users/{id}")
public ResponseEntity<User> getUser(@PathVariable Long id) {
    User user = userService.getUserById(id);
    if (user != null) {
        return ResponseEntity.ok(user);
    }
    return ResponseEntity.notFound().build();
}
```

### Custom Status Code

```java
@PostMapping("/users")
public ResponseEntity<User> createUser(@RequestBody User user) {
    User createdUser = userService.createUser(user);
    return ResponseEntity.status(HttpStatus.CREATED)
            .header("Location", "/api/users/" + createdUser.getId())
            .body(createdUser);
}
```

## HTTP Status Codes

### Common Status Codes

```java
// 200 OK
return ResponseEntity.ok(data);

// 201 Created
return ResponseEntity.status(HttpStatus.CREATED).body(data);

// 204 No Content
return ResponseEntity.noContent().build();

// 400 Bad Request
return ResponseEntity.badRequest().build();

// 404 Not Found
return ResponseEntity.notFound().build();

// 500 Internal Server Error
return ResponseEntity.status(HttpStatus.INTERNAL_SERVER_ERROR).build();
```

## Request Headers

### Reading Headers

```java
@GetMapping("/users")
public List<User> getUsers(@RequestHeader("Authorization") String auth) {
    // Process authorization header
    return userService.getAllUsers();
}

@GetMapping("/users")
public List<User> getUsers(
        @RequestHeader(value = "User-Agent", required = false) String userAgent) {
    // Process user agent
    return userService.getAllUsers();
}
```

### Setting Response Headers

```java
@GetMapping("/users/{id}")
public ResponseEntity<User> getUser(@PathVariable Long id) {
    User user = userService.getUserById(id);
    HttpHeaders headers = new HttpHeaders();
    headers.add("Custom-Header", "value");
    return ResponseEntity.ok().headers(headers).body(user);
}
```

## Content Negotiation

### Produces

```java
@GetMapping(value = "/users", produces = MediaType.APPLICATION_JSON_VALUE)
public List<User> getUsersJson() {
    return userService.getAllUsers();
}

@GetMapping(value = "/users", produces = MediaType.APPLICATION_XML_VALUE)
public List<User> getUsersXml() {
    return userService.getAllUsers();
}
```

### Consumes

```java
@PostMapping(value = "/users", consumes = MediaType.APPLICATION_JSON_VALUE)
public User createUser(@RequestBody User user) {
    return userService.createUser(user);
}
```

## Pagination

### Using Pageable

```java
@GetMapping("/users")
public Page<User> getUsers(Pageable pageable) {
    return userService.getAllUsers(pageable);
}
```

### Custom Pagination

```java
@GetMapping("/users")
public ResponseEntity<Map<String, Object>> getUsers(
        @RequestParam(defaultValue = "0") int page,
        @RequestParam(defaultValue = "10") int size) {
    
    Page<User> userPage = userService.getUsers(page, size);
    
    Map<String, Object> response = new HashMap<>();
    response.put("users", userPage.getContent());
    response.put("currentPage", userPage.getNumber());
    response.put("totalItems", userPage.getTotalElements());
    response.put("totalPages", userPage.getTotalPages());
    
    return ResponseEntity.ok(response);
}
```

## Sorting

```java
@GetMapping("/users")
public List<User> getUsers(
        @RequestParam(defaultValue = "id") String sortBy,
        @RequestParam(defaultValue = "asc") String sortDir) {
    return userService.getUsers(sortBy, sortDir);
}
```

## Filtering

```java
@GetMapping("/users")
public List<User> getUsers(
        @RequestParam(required = false) String name,
        @RequestParam(required = false) String email,
        @RequestParam(required = false) String role) {
    return userService.getUsers(name, email, role);
}
```

## REST API Best Practices

### 1. Use Proper HTTP Methods
- GET for retrieval
- POST for creation
- PUT for full updates
- PATCH for partial updates
- DELETE for deletion

### 2. Use Meaningful URLs
```
✅ Good: /api/users/{id}
❌ Bad: /api/getUser?id=123
```

### 3. Use Proper Status Codes
- 200 OK for successful GET, PUT, PATCH
- 201 Created for successful POST
- 204 No Content for successful DELETE
- 400 Bad Request for validation errors
- 404 Not Found for missing resources
- 500 Internal Server Error for server errors

### 4. Version Your APIs
```
/api/v1/users
/api/v2/users
```

### 5. Use Consistent Response Format

```java
public class ApiResponse<T> {
    private boolean success;
    private String message;
    private T data;
    private List<String> errors;
    // constructors, getters, setters
}
```

### 6. Handle Errors Properly

```java
@ExceptionHandler(ResourceNotFoundException.class)
public ResponseEntity<ApiResponse<?>> handleResourceNotFound(
        ResourceNotFoundException ex) {
    ApiResponse<?> response = new ApiResponse<>(
        false, ex.getMessage(), null, null
    );
    return ResponseEntity.status(HttpStatus.NOT_FOUND).body(response);
}
```

## Complete REST API Example

```java
@RestController
@RequestMapping("/api/v1/users")
public class UserController {
    
    private final UserService userService;
    
    public UserController(UserService userService) {
        this.userService = userService;
    }
    
    @GetMapping
    public ResponseEntity<ApiResponse<List<User>>> getAllUsers(
            @RequestParam(defaultValue = "0") int page,
            @RequestParam(defaultValue = "10") int size) {
        Page<User> users = userService.getAllUsers(page, size);
        ApiResponse<List<User>> response = new ApiResponse<>(
            true, "Users retrieved successfully", users.getContent(), null
        );
        return ResponseEntity.ok(response);
    }
    
    @GetMapping("/{id}")
    public ResponseEntity<ApiResponse<User>> getUser(@PathVariable Long id) {
        User user = userService.getUserById(id);
        ApiResponse<User> response = new ApiResponse<>(
            true, "User retrieved successfully", user, null
        );
        return ResponseEntity.ok(response);
    }
    
    @PostMapping
    public ResponseEntity<ApiResponse<User>> createUser(
            @Valid @RequestBody User user) {
        User createdUser = userService.createUser(user);
        ApiResponse<User> response = new ApiResponse<>(
            true, "User created successfully", createdUser, null
        );
        return ResponseEntity.status(HttpStatus.CREATED).body(response);
    }
    
    @PutMapping("/{id}")
    public ResponseEntity<ApiResponse<User>> updateUser(
            @PathVariable Long id,
            @Valid @RequestBody User user) {
        User updatedUser = userService.updateUser(id, user);
        ApiResponse<User> response = new ApiResponse<>(
            true, "User updated successfully", updatedUser, null
        );
        return ResponseEntity.ok(response);
    }
    
    @DeleteMapping("/{id}")
    public ResponseEntity<ApiResponse<Void>> deleteUser(@PathVariable Long id) {
        userService.deleteUser(id);
        ApiResponse<Void> response = new ApiResponse<>(
            true, "User deleted successfully", null, null
        );
        return ResponseEntity.ok(response);
    }
}
```

## Summary

Spring Boot REST APIs:
- Use `@RestController` for REST endpoints
- Support all HTTP methods (GET, POST, PUT, DELETE, PATCH)
- Handle path variables, query parameters, and request bodies
- Support pagination, sorting, and filtering
- Use proper HTTP status codes
- Follow REST best practices


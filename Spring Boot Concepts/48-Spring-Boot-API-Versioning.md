# Spring Boot API Versioning

## API Versioning Strategies

API versioning allows you to maintain multiple versions of your API simultaneously, enabling backward compatibility and gradual migration.

## URL Path Versioning

### Path-Based Versioning

```java
@RestController
@RequestMapping("/api/v1/users")
public class UserControllerV1 {
    
    @GetMapping
    public ResponseEntity<List<User>> getAllUsers() {
        return ResponseEntity.ok(userService.getAllUsers());
    }
}

@RestController
@RequestMapping("/api/v2/users")
public class UserControllerV2 {
    
    @GetMapping
    public ResponseEntity<List<UserDTO>> getAllUsers() {
        return ResponseEntity.ok(userService.getAllUsersDTO());
    }
}
```

## Header Versioning

### Accept Header Versioning

```java
@RestController
@RequestMapping("/api/users")
public class UserController {
    
    @GetMapping(produces = "application/vnd.api.v1+json")
    public ResponseEntity<List<User>> getAllUsersV1() {
        return ResponseEntity.ok(userService.getAllUsers());
    }
    
    @GetMapping(produces = "application/vnd.api.v2+json")
    public ResponseEntity<List<UserDTO>> getAllUsersV2() {
        return ResponseEntity.ok(userService.getAllUsersDTO());
    }
}
```

## Parameter Versioning

### Query Parameter Versioning

```java
@RestController
@RequestMapping("/api/users")
public class UserController {
    
    @GetMapping(params = "version=1")
    public ResponseEntity<List<User>> getAllUsersV1() {
        return ResponseEntity.ok(userService.getAllUsers());
    }
    
    @GetMapping(params = "version=2")
    public ResponseEntity<List<UserDTO>> getAllUsersV2() {
        return ResponseEntity.ok(userService.getAllUsersDTO());
    }
}
```

## Custom Versioning Strategy

### Version Resolver

```java
public class ApiVersionResolver implements HandlerMethodArgumentResolver {
    
    @Override
    public boolean supportsParameter(MethodParameter parameter) {
        return parameter.getParameterType().equals(ApiVersion.class);
    }
    
    @Override
    public Object resolveArgument(MethodParameter parameter, 
                                  ModelAndViewContainer mavContainer,
                                  NativeWebRequest webRequest, 
                                  WebDataBinderFactory binderFactory) {
        HttpServletRequest request = webRequest.getNativeRequest(HttpServletRequest.class);
        
        // Check path
        String path = request.getRequestURI();
        if (path.contains("/v1/")) {
            return ApiVersion.V1;
        } else if (path.contains("/v2/")) {
            return ApiVersion.V2;
        }
        
        // Check header
        String version = request.getHeader("X-API-Version");
        if ("1".equals(version)) {
            return ApiVersion.V1;
        } else if ("2".equals(version)) {
            return ApiVersion.V2;
        }
        
        return ApiVersion.V1; // Default
    }
}
```

### Version Enum

```java
public enum ApiVersion {
    V1, V2
}
```

### Using Version Resolver

```java
@Configuration
public class WebConfig implements WebMvcConfigurer {
    
    @Override
    public void addArgumentResolvers(List<HandlerMethodArgumentResolver> resolvers) {
        resolvers.add(new ApiVersionResolver());
    }
}

@RestController
@RequestMapping("/api/users")
public class UserController {
    
    @GetMapping
    public ResponseEntity<?> getAllUsers(ApiVersion version) {
        if (version == ApiVersion.V1) {
            return ResponseEntity.ok(userService.getAllUsers());
        } else {
            return ResponseEntity.ok(userService.getAllUsersDTO());
        }
    }
}
```

## Conditional Mapping

### Using @RequestMapping

```java
@RestController
public class UserController {
    
    @GetMapping(value = "/api/users", headers = "X-API-Version=1")
    public ResponseEntity<List<User>> getAllUsersV1() {
        return ResponseEntity.ok(userService.getAllUsers());
    }
    
    @GetMapping(value = "/api/users", headers = "X-API-Version=2")
    public ResponseEntity<List<UserDTO>> getAllUsersV2() {
        return ResponseEntity.ok(userService.getAllUsersDTO());
    }
}
```

## Best Practices

1. **Choose Strategy**: Choose versioning strategy based on requirements
2. **Document Versions**: Document all API versions
3. **Deprecation Policy**: Have a clear deprecation policy
4. **Backward Compatibility**: Maintain backward compatibility
5. **Version Headers**: Return version in response headers

## Summary

Spring Boot API Versioning:
- Multiple versioning strategies
- Maintains backward compatibility
- Enables gradual migration
- Essential for API evolution


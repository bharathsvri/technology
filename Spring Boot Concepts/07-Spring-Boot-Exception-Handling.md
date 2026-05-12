# Spring Boot Exception Handling

## Why Exception Handling?

Proper exception handling in REST APIs ensures:
- Consistent error responses
- Better user experience
- Security (don't expose internal details)
- Easier debugging
- Proper HTTP status codes

## Exception Handling Approaches

### 1. @ExceptionHandler (Controller Level)

Handle exceptions within a specific controller:

```java
@RestController
@RequestMapping("/api/users")
public class UserController {
    
    @ExceptionHandler(ResourceNotFoundException.class)
    public ResponseEntity<ErrorResponse> handleResourceNotFound(
            ResourceNotFoundException ex) {
        ErrorResponse error = new ErrorResponse(
            HttpStatus.NOT_FOUND.value(),
            ex.getMessage(),
            System.currentTimeMillis()
        );
        return ResponseEntity.status(HttpStatus.NOT_FOUND).body(error);
    }
    
    @GetMapping("/{id}")
    public User getUser(@PathVariable Long id) {
        return userService.getUserById(id)
            .orElseThrow(() -> new ResourceNotFoundException("User not found"));
    }
}
```

### 2. @ControllerAdvice (Global Exception Handler)

Handle exceptions across all controllers:

```java
@ControllerAdvice
public class GlobalExceptionHandler {
    
    @ExceptionHandler(ResourceNotFoundException.class)
    public ResponseEntity<ErrorResponse> handleResourceNotFound(
            ResourceNotFoundException ex) {
        ErrorResponse error = new ErrorResponse(
            HttpStatus.NOT_FOUND.value(),
            ex.getMessage(),
            System.currentTimeMillis()
        );
        return ResponseEntity.status(HttpStatus.NOT_FOUND).body(error);
    }
    
    @ExceptionHandler(ValidationException.class)
    public ResponseEntity<ErrorResponse> handleValidation(
            ValidationException ex) {
        ErrorResponse error = new ErrorResponse(
            HttpStatus.BAD_REQUEST.value(),
            ex.getMessage(),
            System.currentTimeMillis()
        );
        return ResponseEntity.status(HttpStatus.BAD_REQUEST).body(error);
    }
}
```

### 3. @RestControllerAdvice

Same as `@ControllerAdvice` but automatically adds `@ResponseBody`:

```java
@RestControllerAdvice
public class GlobalExceptionHandler {
    
    @ExceptionHandler(ResourceNotFoundException.class)
    public ResponseEntity<ErrorResponse> handleResourceNotFound(
            ResourceNotFoundException ex) {
        // Handler implementation
    }
}
```

## Custom Exception Classes

### Basic Custom Exception

```java
public class ResourceNotFoundException extends RuntimeException {
    public ResourceNotFoundException(String message) {
        super(message);
    }
}
```

### Exception with Additional Data

```java
public class BusinessException extends RuntimeException {
    private String errorCode;
    private Map<String, Object> details;
    
    public BusinessException(String message, String errorCode) {
        super(message);
        this.errorCode = errorCode;
        this.details = new HashMap<>();
    }
    
    // Getters and setters
}
```

## Error Response Model

### Simple Error Response

```java
public class ErrorResponse {
    private int status;
    private String message;
    private long timestamp;
    
    public ErrorResponse(int status, String message, long timestamp) {
        this.status = status;
        this.message = message;
        this.timestamp = timestamp;
    }
    
    // Getters and setters
}
```

### Detailed Error Response

```java
public class ErrorResponse {
    private int status;
    private String message;
    private String error;
    private String path;
    private long timestamp;
    private List<FieldError> fieldErrors;
    
    public ErrorResponse(int status, String message, String error, 
                        String path, long timestamp) {
        this.status = status;
        this.message = message;
        this.error = error;
        this.path = path;
        this.timestamp = timestamp;
        this.fieldErrors = new ArrayList<>();
    }
    
    // Getters and setters
    
    public static class FieldError {
        private String field;
        private String message;
        private Object rejectedValue;
        
        // Constructors, getters, setters
    }
}
```

## Handling Specific Exceptions

### Resource Not Found

```java
@ExceptionHandler(ResourceNotFoundException.class)
public ResponseEntity<ErrorResponse> handleResourceNotFound(
        ResourceNotFoundException ex, HttpServletRequest request) {
    ErrorResponse error = new ErrorResponse(
        HttpStatus.NOT_FOUND.value(),
        ex.getMessage(),
        "Resource Not Found",
        request.getRequestURI(),
        System.currentTimeMillis()
    );
    return ResponseEntity.status(HttpStatus.NOT_FOUND).body(error);
}
```

### Validation Errors

```java
@ExceptionHandler(MethodArgumentNotValidException.class)
public ResponseEntity<ErrorResponse> handleValidationErrors(
        MethodArgumentNotValidException ex, HttpServletRequest request) {
    
    List<FieldError> fieldErrors = ex.getBindingResult()
        .getFieldErrors()
        .stream()
        .map(error -> new FieldError(
            error.getField(),
            error.getDefaultMessage(),
            error.getRejectedValue()
        ))
        .collect(Collectors.toList());
    
    ErrorResponse error = new ErrorResponse(
        HttpStatus.BAD_REQUEST.value(),
        "Validation failed",
        "Bad Request",
        request.getRequestURI(),
        System.currentTimeMillis()
    );
    error.setFieldErrors(fieldErrors);
    
    return ResponseEntity.status(HttpStatus.BAD_REQUEST).body(error);
}
```

### Constraint Violation

```java
@ExceptionHandler(ConstraintViolationException.class)
public ResponseEntity<ErrorResponse> handleConstraintViolation(
        ConstraintViolationException ex, HttpServletRequest request) {
    
    List<FieldError> fieldErrors = ex.getConstraintViolations()
        .stream()
        .map(violation -> new FieldError(
            violation.getPropertyPath().toString(),
            violation.getMessage(),
            violation.getInvalidValue()
        ))
        .collect(Collectors.toList());
    
    ErrorResponse error = new ErrorResponse(
        HttpStatus.BAD_REQUEST.value(),
        "Validation failed",
        "Bad Request",
        request.getRequestURI(),
        System.currentTimeMillis()
    );
    error.setFieldErrors(fieldErrors);
    
    return ResponseEntity.status(HttpStatus.BAD_REQUEST).body(error);
}
```

### Method Argument Type Mismatch

```java
@ExceptionHandler(MethodArgumentTypeMismatchException.class)
public ResponseEntity<ErrorResponse> handleTypeMismatch(
        MethodArgumentTypeMismatchException ex, HttpServletRequest request) {
    String message = String.format(
        "'%s' should be a valid '%s'",
        ex.getName(),
        ex.getRequiredType().getSimpleName()
    );
    
    ErrorResponse error = new ErrorResponse(
        HttpStatus.BAD_REQUEST.value(),
        message,
        "Bad Request",
        request.getRequestURI(),
        System.currentTimeMillis()
    );
    
    return ResponseEntity.status(HttpStatus.BAD_REQUEST).body(error);
}
```

### Http Request Method Not Supported

```java
@ExceptionHandler(HttpRequestMethodNotSupportedException.class)
public ResponseEntity<ErrorResponse> handleMethodNotSupported(
        HttpRequestMethodNotSupportedException ex, HttpServletRequest request) {
    String message = String.format(
        "Method '%s' is not supported. Supported methods: %s",
        ex.getMethod(),
        Arrays.toString(ex.getSupportedMethods())
    );
    
    ErrorResponse error = new ErrorResponse(
        HttpStatus.METHOD_NOT_ALLOWED.value(),
        message,
        "Method Not Allowed",
        request.getRequestURI(),
        System.currentTimeMillis()
    );
    
    return ResponseEntity.status(HttpStatus.METHOD_NOT_ALLOWED).body(error);
}
```

### Generic Exception Handler

```java
@ExceptionHandler(Exception.class)
public ResponseEntity<ErrorResponse> handleGenericException(
        Exception ex, HttpServletRequest request) {
    
    // Log the exception
    logger.error("Unexpected error occurred", ex);
    
    ErrorResponse error = new ErrorResponse(
        HttpStatus.INTERNAL_SERVER_ERROR.value(),
        "An unexpected error occurred",
        "Internal Server Error",
        request.getRequestURI(),
        System.currentTimeMillis()
    );
    
    return ResponseEntity.status(HttpStatus.INTERNAL_SERVER_ERROR).body(error);
}
```

## Complete Global Exception Handler

```java
@RestControllerAdvice
public class GlobalExceptionHandler {
    
    private static final Logger logger = LoggerFactory.getLogger(GlobalExceptionHandler.class);
    
    @ExceptionHandler(ResourceNotFoundException.class)
    public ResponseEntity<ErrorResponse> handleResourceNotFound(
            ResourceNotFoundException ex, HttpServletRequest request) {
        ErrorResponse error = buildErrorResponse(
            HttpStatus.NOT_FOUND,
            ex.getMessage(),
            "Resource Not Found",
            request
        );
        return ResponseEntity.status(HttpStatus.NOT_FOUND).body(error);
    }
    
    @ExceptionHandler(ValidationException.class)
    public ResponseEntity<ErrorResponse> handleValidation(
            ValidationException ex, HttpServletRequest request) {
        ErrorResponse error = buildErrorResponse(
            HttpStatus.BAD_REQUEST,
            ex.getMessage(),
            "Validation Error",
            request
        );
        return ResponseEntity.status(HttpStatus.BAD_REQUEST).body(error);
    }
    
    @ExceptionHandler(MethodArgumentNotValidException.class)
    public ResponseEntity<ErrorResponse> handleValidationErrors(
            MethodArgumentNotValidException ex, HttpServletRequest request) {
        
        List<FieldError> fieldErrors = ex.getBindingResult()
            .getFieldErrors()
            .stream()
            .map(error -> new FieldError(
                error.getField(),
                error.getDefaultMessage(),
                error.getRejectedValue()
            ))
            .collect(Collectors.toList());
        
        ErrorResponse error = buildErrorResponse(
            HttpStatus.BAD_REQUEST,
            "Validation failed",
            "Bad Request",
            request
        );
        error.setFieldErrors(fieldErrors);
        
        return ResponseEntity.status(HttpStatus.BAD_REQUEST).body(error);
    }
    
    @ExceptionHandler(Exception.class)
    public ResponseEntity<ErrorResponse> handleGenericException(
            Exception ex, HttpServletRequest request) {
        logger.error("Unexpected error occurred", ex);
        
        ErrorResponse error = buildErrorResponse(
            HttpStatus.INTERNAL_SERVER_ERROR,
            "An unexpected error occurred",
            "Internal Server Error",
            request
        );
        
        return ResponseEntity.status(HttpStatus.INTERNAL_SERVER_ERROR).body(error);
    }
    
    private ErrorResponse buildErrorResponse(
            HttpStatus status, String message, String error, 
            HttpServletRequest request) {
        return new ErrorResponse(
            status.value(),
            message,
            error,
            request.getRequestURI(),
            System.currentTimeMillis()
        );
    }
}
```

## Scoped Exception Handlers

### Controller-Specific Handler

```java
@ControllerAdvice(assignableTypes = UserController.class)
public class UserControllerExceptionHandler {
    // Handles exceptions only from UserController
}
```

### Package-Specific Handler

```java
@ControllerAdvice(basePackages = "com.example.controllers")
public class PackageExceptionHandler {
    // Handles exceptions from controllers in specified package
}
```

### Annotation-Specific Handler

```java
@ControllerAdvice(annotations = RestController.class)
public class RestControllerExceptionHandler {
    // Handles exceptions from all @RestController classes
}
```

## Response Status Annotation

### Using @ResponseStatus

```java
@ResponseStatus(HttpStatus.NOT_FOUND)
public class ResourceNotFoundException extends RuntimeException {
    public ResourceNotFoundException(String message) {
        super(message);
    }
}
```

## Exception Handling Best Practices

1. **Use Global Exception Handler**: Centralize exception handling
2. **Create Custom Exceptions**: Use meaningful exception classes
3. **Return Consistent Format**: Use consistent error response structure
4. **Log Exceptions**: Always log exceptions for debugging
5. **Don't Expose Internal Details**: Don't expose stack traces in production
6. **Use Appropriate Status Codes**: Return correct HTTP status codes
7. **Handle Validation Errors**: Properly handle validation exceptions
8. **Document Exceptions**: Document possible exceptions in API documentation

## Error Response Example

```json
{
  "status": 404,
  "message": "User not found with id: 123",
  "error": "Resource Not Found",
  "path": "/api/users/123",
  "timestamp": 1699123456789,
  "fieldErrors": []
}
```

## Validation Error Response Example

```json
{
  "status": 400,
  "message": "Validation failed",
  "error": "Bad Request",
  "path": "/api/users",
  "timestamp": 1699123456789,
  "fieldErrors": [
    {
      "field": "email",
      "message": "Email should be valid",
      "rejectedValue": "invalid-email"
    },
    {
      "field": "age",
      "message": "Age must be positive",
      "rejectedValue": -5
    }
  ]
}
```

## Summary

Spring Boot Exception Handling:
- Use `@ControllerAdvice` or `@RestControllerAdvice` for global handling
- Create custom exception classes
- Return consistent error response format
- Use appropriate HTTP status codes
- Handle validation errors properly
- Log exceptions for debugging
- Don't expose internal details in production

## See also

- [Problem Details (RFC 9457)](62-Spring-Boot-Problem-Details-RFC9457.md) for standardized HTTP error bodies with `ProblemDetail`.

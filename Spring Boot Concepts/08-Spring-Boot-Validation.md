# Spring Boot Validation

## What is Validation?

Validation ensures that data meets certain criteria before processing. In Spring Boot, validation is typically done using Bean Validation (JSR-303/JSR-380) with Hibernate Validator.

## Setting Up Validation

### Add Dependency

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-validation</artifactId>
</dependency>
```

### For Gradle

```gradle
implementation 'org.springframework.boot:spring-boot-starter-validation'
```

## Validation Annotations

### @NotNull
Ensures field is not null:

```java
public class User {
    @NotNull(message = "Name cannot be null")
    private String name;
}
```

### @NotEmpty
Ensures field is not null and not empty:

```java
@NotEmpty(message = "Email cannot be empty")
private String email;
```

### @NotBlank
Ensures string is not null, not empty, and not just whitespace:

```java
@NotBlank(message = "Username cannot be blank")
private String username;
```

### @Size
Validates size of collection, array, or string:

```java
@Size(min = 2, max = 50, message = "Name must be between 2 and 50 characters")
private String name;

@Size(min = 1, max = 10, message = "List must have between 1 and 10 items")
private List<String> items;
```

### @Min and @Max
Validates numeric values:

```java
@Min(value = 18, message = "Age must be at least 18")
@Max(value = 100, message = "Age must be at most 100")
private int age;
```

### @DecimalMin and @DecimalMax
Validates decimal values:

```java
@DecimalMin(value = "0.0", inclusive = false, message = "Price must be greater than 0")
@DecimalMax(value = "10000.0", message = "Price must be less than 10000")
private BigDecimal price;
```

### @Email
Validates email format:

```java
@Email(message = "Email should be valid")
private String email;
```

### @Pattern
Validates string against regex pattern:

```java
@Pattern(regexp = "^[0-9]{10}$", message = "Phone number must be 10 digits")
private String phoneNumber;

@Pattern(regexp = "^(?=.*[a-z])(?=.*[A-Z])(?=.*\\d).{8,}$", 
         message = "Password must contain uppercase, lowercase, and number")
private String password;
```

### @Past and @PastOrPresent
Validates date is in the past:

```java
@Past(message = "Birth date must be in the past")
private LocalDate birthDate;

@PastOrPresent(message = "Date must be in the past or present")
private LocalDate date;
```

### @Future and @FutureOrPresent
Validates date is in the future:

```java
@Future(message = "Event date must be in the future")
private LocalDate eventDate;

@FutureOrPresent(message = "Date must be in the future or present")
private LocalDate date;
```

### @Positive and @PositiveOrZero
Validates positive numbers:

```java
@Positive(message = "Quantity must be positive")
private int quantity;

@PositiveOrZero(message = "Score must be positive or zero")
private int score;
```

### @Negative and @NegativeOrZero
Validates negative numbers:

```java
@Negative(message = "Value must be negative")
private int value;
```

### @AssertTrue and @AssertFalse
Validates boolean values:

```java
@AssertTrue(message = "Must accept terms and conditions")
private boolean acceptTerms;
```

## Validating Request Body

### Using @Valid

```java
@PostMapping("/users")
public ResponseEntity<User> createUser(@Valid @RequestBody User user) {
    return ResponseEntity.ok(userService.createUser(user));
}
```

### Using @Validated

```java
@PostMapping("/users")
public ResponseEntity<User> createUser(@Validated @RequestBody User user) {
    return ResponseEntity.ok(userService.createUser(user));
}
```

## Complete Validation Example

### Entity Class

```java
public class User {
    @NotNull(message = "ID cannot be null")
    private Long id;
    
    @NotBlank(message = "Name is required")
    @Size(min = 2, max = 50, message = "Name must be between 2 and 50 characters")
    private String name;
    
    @NotBlank(message = "Email is required")
    @Email(message = "Email should be valid")
    private String email;
    
    @NotNull(message = "Age is required")
    @Min(value = 18, message = "Age must be at least 18")
    @Max(value = 100, message = "Age must be at most 100")
    private Integer age;
    
    @Pattern(regexp = "^[0-9]{10}$", message = "Phone number must be 10 digits")
    private String phoneNumber;
    
    @Past(message = "Birth date must be in the past")
    private LocalDate birthDate;
    
    // Constructors, getters, setters
}
```

## Handling Validation Errors

### Method 1: Using BindingResult

```java
@PostMapping("/users")
public ResponseEntity<?> createUser(
        @Valid @RequestBody User user,
        BindingResult bindingResult) {
    
    if (bindingResult.hasErrors()) {
        List<String> errors = bindingResult.getFieldErrors()
            .stream()
            .map(FieldError::getDefaultMessage)
            .collect(Collectors.toList());
        return ResponseEntity.badRequest().body(errors);
    }
    
    return ResponseEntity.ok(userService.createUser(user));
}
```

### Method 2: Using Exception Handler (Recommended)

```java
@RestControllerAdvice
public class GlobalExceptionHandler {
    
    @ExceptionHandler(MethodArgumentNotValidException.class)
    public ResponseEntity<ErrorResponse> handleValidationErrors(
            MethodArgumentNotValidException ex) {
        
        Map<String, String> errors = new HashMap<>();
        ex.getBindingResult().getFieldErrors().forEach(error ->
            errors.put(error.getField(), error.getDefaultMessage())
        );
        
        ErrorResponse errorResponse = new ErrorResponse(
            HttpStatus.BAD_REQUEST.value(),
            "Validation failed",
            errors,
            System.currentTimeMillis()
        );
        
        return ResponseEntity.status(HttpStatus.BAD_REQUEST).body(errorResponse);
    }
}
```

## Nested Object Validation

### Validating Nested Objects

```java
public class User {
    @NotBlank(message = "Name is required")
    private String name;
    
    @Valid
    @NotNull(message = "Address is required")
    private Address address;
}

public class Address {
    @NotBlank(message = "Street is required")
    private String street;
    
    @NotBlank(message = "City is required")
    private String city;
    
    @Pattern(regexp = "^[0-9]{5}$", message = "Zip code must be 5 digits")
    private String zipCode;
}
```

## Collection Validation

### Validating Collections

```java
@NotEmpty(message = "Tags cannot be empty")
@Size(min = 1, max = 10, message = "Must have between 1 and 10 tags")
private List<@NotBlank(message = "Tag cannot be blank") String> tags;
```

## Custom Validation

### Creating Custom Validator

```java
@Target({ElementType.FIELD})
@Retention(RetentionPolicy.RUNTIME)
@Constraint(validatedBy = PhoneNumberValidator.class)
public @interface ValidPhoneNumber {
    String message() default "Invalid phone number";
    Class<?>[] groups() default {};
    Class<? extends Payload>[] payload() default {};
}
```

### Validator Implementation

```java
public class PhoneNumberValidator implements ConstraintValidator<ValidPhoneNumber, String> {
    
    @Override
    public void initialize(ValidPhoneNumber constraintAnnotation) {
    }
    
    @Override
    public boolean isValid(String phoneNumber, ConstraintValidatorContext context) {
        if (phoneNumber == null) {
            return true; // Use @NotNull for null checks
        }
        return phoneNumber.matches("^[0-9]{10}$");
    }
}
```

### Using Custom Validator

```java
public class User {
    @ValidPhoneNumber(message = "Invalid phone number format")
    private String phoneNumber;
}
```

## Group Validation

### Defining Validation Groups

```java
public interface CreateGroup {}
public interface UpdateGroup {}
```

### Using Groups

```java
public class User {
    @NotNull(groups = UpdateGroup.class, message = "ID is required for update")
    private Long id;
    
    @NotBlank(groups = {CreateGroup.class, UpdateGroup.class}, message = "Name is required")
    private String name;
}
```

### Validating with Groups

```java
@PostMapping("/users")
public ResponseEntity<User> createUser(
        @Validated(CreateGroup.class) @RequestBody User user) {
    return ResponseEntity.ok(userService.createUser(user));
}

@PutMapping("/users/{id}")
public ResponseEntity<User> updateUser(
        @PathVariable Long id,
        @Validated(UpdateGroup.class) @RequestBody User user) {
    return ResponseEntity.ok(userService.updateUser(id, user));
}
```

## Method-Level Validation

### Validating Method Parameters

```java
@Service
@Validated
public class UserService {
    
    public User getUserById(@Min(1) Long id) {
        // Implementation
    }
    
    public void updateUser(@Valid User user) {
        // Implementation
    }
}
```

### Validating Return Values

```java
@Validated
public class UserService {
    
    @Valid
    public User createUser(@Valid User user) {
        // Implementation
        return user;
    }
}
```

## Conditional Validation

### Using @ConditionalOnProperty

```java
@ConditionalOnProperty(name = "validation.enabled", havingValue = "true")
@Configuration
public class ValidationConfig {
    // Configuration
}
```

## Validation Messages

### Custom Messages in Properties File

Create `ValidationMessages.properties`:

```properties
user.name.required=Name is required
user.email.invalid=Email should be valid
user.age.min=Age must be at least {value}
user.age.max=Age must be at most {value}
```

### Using in Annotations

```java
@NotBlank(message = "{user.name.required}")
private String name;

@Email(message = "{user.email.invalid}")
private String email;

@Min(value = 18, message = "{user.age.min}")
private Integer age;
```

## Validation Best Practices

1. **Use Appropriate Annotations**: Choose the right validation annotation
2. **Provide Clear Messages**: Use descriptive error messages
3. **Validate Early**: Validate at the controller level
4. **Handle Errors Properly**: Use exception handlers for validation errors
5. **Use Groups**: Use validation groups for different scenarios
6. **Custom Validators**: Create custom validators for complex validation
7. **Validate Nested Objects**: Use `@Valid` for nested object validation
8. **Test Validation**: Write tests for validation logic

## Complete Example

### User Entity

```java
public class User {
    @NotNull(groups = UpdateGroup.class)
    private Long id;
    
    @NotBlank(groups = {CreateGroup.class, UpdateGroup.class})
    @Size(min = 2, max = 50)
    private String name;
    
    @NotBlank(groups = {CreateGroup.class, UpdateGroup.class})
    @Email
    private String email;
    
    @NotNull(groups = {CreateGroup.class, UpdateGroup.class})
    @Min(18)
    @Max(100)
    private Integer age;
    
    @Valid
    private Address address;
    
    // Constructors, getters, setters
}
```

### Controller

```java
@RestController
@RequestMapping("/api/users")
public class UserController {
    
    @PostMapping
    public ResponseEntity<User> createUser(
            @Validated(CreateGroup.class) @RequestBody User user) {
        return ResponseEntity.ok(userService.createUser(user));
    }
    
    @PutMapping("/{id}")
    public ResponseEntity<User> updateUser(
            @PathVariable Long id,
            @Validated(UpdateGroup.class) @RequestBody User user) {
        return ResponseEntity.ok(userService.updateUser(id, user));
    }
}
```

## Summary

Spring Boot Validation:
- Uses Bean Validation (JSR-303/JSR-380)
- Provides built-in validation annotations
- Supports custom validators
- Handles validation errors via exception handlers
- Supports validation groups
- Validates nested objects and collections
- Provides method-level validation


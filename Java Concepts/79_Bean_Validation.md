# Bean Validation

## What is Bean Validation?
Bean Validation (JSR 303/380) provides annotations for validating Java objects.

## Basic Annotations
```java
import javax.validation.constraints.*;

public class User {
    @NotNull
    @Size(min = 2, max = 50)
    private String username;
    
    @NotNull
    @Email
    private String email;
    
    @Min(18)
    @Max(120)
    private Integer age;
    
    @NotNull
    @Pattern(regexp = "^[A-Z]{2}\\d{2}[A-Z0-9]{4}\\d{7}$")
    private String iban;
    
    @NotNull
    @DecimalMin(value = "0.0", inclusive = false)
    @DecimalMax(value = "1000000.0")
    private BigDecimal salary;
    
    @Past
    private Date birthDate;
    
    @Future
    private Date expirationDate;
    
    // Getters and setters
}
```

## Validating Objects
```java
import javax.validation.*;

public class ValidationExample {
    public static void main(String[] args) {
        ValidatorFactory factory = Validation.buildDefaultValidatorFactory();
        Validator validator = factory.getValidator();
        
        User user = new User();
        user.setUsername("a"); // Too short
        user.setEmail("invalid-email"); // Invalid format
        user.setAge(15); // Too young
        
        Set<ConstraintViolation<User>> violations = validator.validate(user);
        
        for (ConstraintViolation<User> violation : violations) {
            System.out.println(violation.getPropertyPath() + ": " + 
                             violation.getMessage());
        }
    }
}
```

## Custom Validator
```java
import javax.validation.*;

@Target({ElementType.FIELD})
@Retention(RetentionPolicy.RUNTIME)
@Constraint(validatedBy = PhoneNumberValidator.class)
public @interface PhoneNumber {
    String message() default "Invalid phone number";
    Class<?>[] groups() default {};
    Class<? extends Payload>[] payload() default {};
}

class PhoneNumberValidator implements ConstraintValidator<PhoneNumber, String> {
    @Override
    public void initialize(PhoneNumber constraintAnnotation) {
    }
    
    @Override
    public boolean isValid(String value, ConstraintValidatorContext context) {
        if (value == null) {
            return true; // Use @NotNull for null checks
        }
        return value.matches("^\\+?[1-9]\\d{1,14}$");
    }
}
```

## Method Validation
```java
import javax.validation.constraints.*;

public class UserService {
    @Valid
    public void createUser(@NotNull @Valid User user) {
        // Create user
    }
    
    public User getUser(@NotNull @Min(1) Long id) {
        // Get user
        return user;
    }
}
```

## Spring Integration
```java
import org.springframework.validation.annotation.Validated;
import org.springframework.web.bind.annotation.*;

@RestController
@RequestMapping("/api/users")
@Validated
public class UserController {
    
    @PostMapping
    public ResponseEntity<User> createUser(@Valid @RequestBody User user) {
        // Create user
        return ResponseEntity.ok(user);
    }
    
    @GetMapping("/{id}")
    public ResponseEntity<User> getUser(
            @PathVariable @Min(1) Long id) {
        // Get user
        return ResponseEntity.ok(user);
    }
}
```

## Best Practices
1. Validate at boundaries
2. Use appropriate constraints
3. Create custom validators for complex rules
4. Validate in service layer
5. Return clear error messages
6. Use groups for conditional validation
7. Validate collections
8. Test validation logic


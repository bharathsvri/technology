# Spring Boot Command Line Runner

## What are Command Line Runners?

Command Line Runners are interfaces that allow you to execute code after the Spring Boot application has started. They're useful for initialization tasks, data seeding, and startup operations.

## CommandLineRunner

### Basic Implementation

```java
@Component
public class DataInitializer implements CommandLineRunner {
    
    private final UserRepository userRepository;
    
    public DataInitializer(UserRepository userRepository) {
        this.userRepository = userRepository;
    }
    
    @Override
    public void run(String... args) throws Exception {
        System.out.println("Initializing data...");
        
        if (userRepository.count() == 0) {
            User admin = new User("Admin", "admin@example.com");
            userRepository.save(admin);
            System.out.println("Admin user created");
        }
    }
}
```

### Multiple Runners

```java
@Component
@Order(1)
public class FirstRunner implements CommandLineRunner {
    
    @Override
    public void run(String... args) throws Exception {
        System.out.println("First runner executed");
    }
}

@Component
@Order(2)
public class SecondRunner implements CommandLineRunner {
    
    @Override
    public void run(String... args) throws Exception {
        System.out.println("Second runner executed");
    }
}
```

## ApplicationRunner

### Using ApplicationRunner

```java
@Component
public class ApplicationInitializer implements ApplicationRunner {
    
    private final UserService userService;
    
    public ApplicationInitializer(UserService userService) {
        this.userService = userService;
    }
    
    @Override
    public void run(ApplicationArguments args) throws Exception {
        System.out.println("Application started with arguments:");
        args.getOptionNames().forEach(name -> {
            System.out.println(name + " = " + args.getOptionValues(name));
        });
        
        // Initialization logic
        initializeData();
    }
    
    private void initializeData() {
        // Data initialization
    }
}
```

## Accessing Command Line Arguments

### Using CommandLineRunner

```java
@Component
public class ArgumentProcessor implements CommandLineRunner {
    
    @Override
    public void run(String... args) throws Exception {
        for (String arg : args) {
            System.out.println("Argument: " + arg);
        }
    }
}
```

### Using ApplicationRunner

```java
@Component
public class ArgumentProcessor implements ApplicationRunner {
    
    @Override
    public void run(ApplicationArguments args) throws Exception {
        // Non-option arguments
        List<String> nonOptionArgs = args.getNonOptionArgs();
        nonOptionArgs.forEach(arg -> System.out.println("Non-option: " + arg));
        
        // Option arguments
        Set<String> optionNames = args.getOptionNames();
        optionNames.forEach(name -> {
            List<String> values = args.getOptionValues(name);
            System.out.println(name + " = " + values);
        });
    }
}
```

## Conditional Execution

### Using @ConditionalOnProperty

```java
@Component
@ConditionalOnProperty(name = "app.init.enabled", havingValue = "true")
public class DataInitializer implements CommandLineRunner {
    
    @Override
    public void run(String... args) throws Exception {
        // Only runs if app.init.enabled=true
    }
}
```

### Using @Profile

```java
@Component
@Profile("dev")
public class DevDataInitializer implements CommandLineRunner {
    
    @Override
    public void run(String... args) throws Exception {
        // Only runs in dev profile
    }
}
```

## Complete Example

### Data Seeding

```java
@Component
@Order(1)
public class DataSeeder implements CommandLineRunner {
    
    private final UserRepository userRepository;
    private final RoleRepository roleRepository;
    
    public DataSeeder(UserRepository userRepository, RoleRepository roleRepository) {
        this.userRepository = userRepository;
        this.roleRepository = roleRepository;
    }
    
    @Override
    public void run(String... args) throws Exception {
        if (userRepository.count() == 0) {
            seedUsers();
        }
        
        if (roleRepository.count() == 0) {
            seedRoles();
        }
    }
    
    private void seedUsers() {
        User admin = new User("Admin", "admin@example.com");
        admin.setPassword(passwordEncoder.encode("admin123"));
        userRepository.save(admin);
    }
    
    private void seedRoles() {
        roleRepository.save(new Role("ADMIN"));
        roleRepository.save(new Role("USER"));
    }
}
```

### Configuration Validator

```java
@Component
@Order(2)
public class ConfigurationValidator implements ApplicationRunner {
    
    @Value("${app.required.property}")
    private String requiredProperty;
    
    @Override
    public void run(ApplicationArguments args) throws Exception {
        if (requiredProperty == null || requiredProperty.isEmpty()) {
            throw new IllegalStateException("Required property is not set");
        }
        
        System.out.println("Configuration validated successfully");
    }
}
```

## Best Practices

1. **Use @Order**: Use @Order to control execution order
2. **Check Conditions**: Check if initialization is needed
3. **Handle Errors**: Handle errors gracefully
4. **Use Profiles**: Use profiles for environment-specific initialization
5. **Keep Fast**: Keep initialization fast

## Summary

Spring Boot Command Line Runners:
- Execute code after application startup
- Useful for initialization tasks
- Support command line arguments
- Essential for application setup


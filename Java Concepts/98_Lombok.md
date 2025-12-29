# Lombok

## What is Lombok?
Lombok reduces boilerplate code by generating getters, setters, constructors, and more at compile time.

## Setup
```xml
<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
    <version>1.18.30</version>
    <scope>provided</scope>
</dependency>
```

## @Getter and @Setter
```java
import lombok.*;

@Getter
@Setter
public class User {
    private String name;
    private int age;
    private String email;
}

// Automatically generates:
// getName(), setName(), getAge(), setAge(), getEmail(), setEmail()
```

## @Data
```java
@Data
public class User {
    private String name;
    private int age;
}

// Generates: getters, setters, toString(), equals(), hashCode()
```

## @Builder
```java
@Builder
public class User {
    private String name;
    private int age;
    private String email;
}

// Usage
User user = User.builder()
    .name("John")
    .age(25)
    .email("john@example.com")
    .build();
```

## @AllArgsConstructor and @NoArgsConstructor
```java
@AllArgsConstructor
@NoArgsConstructor
public class User {
    private String name;
    private int age;
}

// Generates constructors with all args and no args
```

## @ToString
```java
@ToString(exclude = "password")
public class User {
    private String name;
    private String password;
}

// Generates toString() excluding password
```

## @EqualsAndHashCode
```java
@EqualsAndHashCode(exclude = "id")
public class User {
    private Long id;
    private String name;
}

// Generates equals() and hashCode() excluding id
```

## @Slf4j
```java
@Slf4j
public class MyClass {
    public void method() {
        log.info("Logging message");
        log.error("Error message", exception);
    }
}

// Automatically creates: private static final Logger log = LoggerFactory.getLogger(MyClass.class);
```

## @SneakyThrows
```java
@SneakyThrows
public void method() {
    Files.readAllLines(Paths.get("file.txt"));
    // No need to handle IOException
}
```

## @Value (Immutable)
```java
@Value
public class User {
    String name;
    int age;
}

// Creates immutable class with final fields, getters, equals, hashCode, toString
```

## Best Practices
1. Use appropriate annotations
2. Don't overuse Lombok
3. Exclude sensitive fields from toString/equals
4. Use @Builder for complex objects
5. Use @Value for immutable objects
6. Keep Lombok updated
7. Understand generated code
8. Use IDE plugin for better support


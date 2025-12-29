# Spring Boot Custom Auto Configuration

## Creating Custom Auto Configuration

Custom auto-configuration allows you to create reusable Spring Boot starters that automatically configure beans based on classpath and properties.

## Project Structure

```
custom-starter/
├── src/
│   ├── main/
│   │   ├── java/
│   │   │   └── com/
│   │   │       └── example/
│   │   │           ├── CustomService.java
│   │   │           ├── CustomProperties.java
│   │   │           └── CustomAutoConfiguration.java
│   │   └── resources/
│   │       └── META-INF/
│   │           └── spring.factories
│   └── test/
```

## Custom Service

### Service Interface

```java
public interface CustomService {
    String process(String input);
}
```

### Service Implementation

```java
public class CustomServiceImpl implements CustomService {
    
    private final CustomProperties properties;
    
    public CustomServiceImpl(CustomProperties properties) {
        this.properties = properties;
    }
    
    @Override
    public String process(String input) {
        return properties.getPrefix() + input + properties.getSuffix();
    }
}
```

## Configuration Properties

### Properties Class

```java
@ConfigurationProperties(prefix = "custom")
public class CustomProperties {
    private String prefix = "[";
    private String suffix = "]";
    private boolean enabled = true;
    
    // Getters and setters
}
```

## Auto Configuration Class

### Auto Configuration

```java
@Configuration
@ConditionalOnClass(CustomService.class)
@ConditionalOnProperty(prefix = "custom", name = "enabled", havingValue = "true", matchIfMissing = true)
@EnableConfigurationProperties(CustomProperties.class)
public class CustomAutoConfiguration {
    
    @Bean
    @ConditionalOnMissingBean
    public CustomService customService(CustomProperties properties) {
        return new CustomServiceImpl(properties);
    }
}
```

## Registering Auto Configuration

### spring.factories

Create `META-INF/spring.factories`:

```properties
org.springframework.boot.autoconfigure.EnableAutoConfiguration=\
com.example.CustomAutoConfiguration
```

## Conditional Annotations

### Using Conditionals

```java
@Configuration
@ConditionalOnClass(CustomService.class)
@ConditionalOnProperty(name = "custom.enabled", havingValue = "true")
@ConditionalOnMissingBean(CustomService.class)
@AutoConfigureAfter(DataSourceAutoConfiguration.class)
public class CustomAutoConfiguration {
    // Configuration
}
```

## Starter Module

### Starter POM

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project>
    <modelVersion>4.0.0</modelVersion>
    
    <groupId>com.example</groupId>
    <artifactId>custom-spring-boot-starter</artifactId>
    <version>1.0.0</version>
    
    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-autoconfigure</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-configuration-processor</artifactId>
            <optional>true</optional>
        </dependency>
    </dependencies>
</project>
```

## Using Custom Starter

### Add Dependency

```xml
<dependency>
    <groupId>com.example</groupId>
    <artifactId>custom-spring-boot-starter</artifactId>
    <version>1.0.0</version>
</dependency>
```

### Configuration

```properties
custom.enabled=true
custom.prefix=Hello
custom.suffix=World
```

### Using Service

```java
@Service
public class MyService {
    
    private final CustomService customService;
    
    public MyService(CustomService customService) {
        this.customService = customService;
    }
    
    public void doSomething() {
        String result = customService.process("test");
        System.out.println(result); // Output: Hello test World
    }
}
```

## Best Practices

1. **Use Conditional Annotations**: Use appropriate conditionals
2. **Provide Defaults**: Provide sensible defaults
3. **Document Properties**: Document all configuration properties
4. **Test Auto Configuration**: Test with and without dependencies
5. **Use @ConditionalOnMissingBean**: Allow overrides

## Summary

Spring Boot Custom Auto Configuration:
- Create reusable starters
- Automatic configuration
- Conditional bean creation
- Essential for creating libraries


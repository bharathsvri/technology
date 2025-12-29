# Spring Boot Conditional Beans

## What are Conditional Beans?

Conditional beans are beans that are only created when certain conditions are met. Spring Boot provides various conditional annotations for this purpose.

## Conditional Annotations

### @ConditionalOnProperty

```java
@Configuration
public class FeatureConfig {
    
    @Bean
    @ConditionalOnProperty(name = "feature.email.enabled", havingValue = "true")
    public EmailService emailService() {
        return new EmailService();
    }
    
    @Bean
    @ConditionalOnProperty(name = "feature.sms.enabled", havingValue = "true", matchIfMissing = true)
    public SmsService smsService() {
        return new SmsService();
    }
}
```

### @ConditionalOnClass

```java
@Configuration
@ConditionalOnClass(DataSource.class)
public class DatabaseConfig {
    
    @Bean
    public DataSource dataSource() {
        // DataSource configuration
    }
}
```

### @ConditionalOnMissingBean

```java
@Configuration
public class ServiceConfig {
    
    @Bean
    @ConditionalOnMissingBean
    public UserService defaultUserService() {
        return new DefaultUserService();
    }
}
```

### @ConditionalOnBean

```java
@Configuration
public class ServiceConfig {
    
    @Bean
    @ConditionalOnBean(DataSource.class)
    public UserRepository userRepository(DataSource dataSource) {
        return new JdbcUserRepository(dataSource);
    }
}
```

### @ConditionalOnProfile

```java
@Configuration
@Profile("dev")
public class DevConfig {
    
    @Bean
    public DataSource devDataSource() {
        return new EmbeddedDatabaseBuilder()
            .setType(EmbeddedDatabaseType.H2)
            .build();
    }
}

@Configuration
@Profile("prod")
public class ProdConfig {
    
    @Bean
    public DataSource prodDataSource() {
        // Production DataSource configuration
    }
}
```

### @ConditionalOnExpression

```java
@Configuration
public class ConditionalConfig {
    
    @Bean
    @ConditionalOnExpression("${feature.advanced.enabled:false} and ${feature.basic.enabled:true}")
    public AdvancedService advancedService() {
        return new AdvancedService();
    }
}
```

### @ConditionalOnWebApplication

```java
@Configuration
@ConditionalOnWebApplication
public class WebConfig {
    
    @Bean
    public WebMvcConfigurer webMvcConfigurer() {
        return new WebMvcConfigurer() {
            // Configuration
        };
    }
}
```

### @ConditionalOnNotWebApplication

```java
@Configuration
@ConditionalOnNotWebApplication
public class BatchConfig {
    
    @Bean
    public JobLauncher jobLauncher() {
        // Batch job launcher configuration
    }
}
```

## Custom Conditions

### Creating Custom Condition

```java
public class CustomCondition implements Condition {
    
    @Override
    public boolean matches(ConditionContext context, AnnotatedTypeMetadata metadata) {
        Environment env = context.getEnvironment();
        return env.getProperty("custom.feature.enabled", Boolean.class, false);
    }
}
```

### Using Custom Condition

```java
@Configuration
public class CustomConfig {
    
    @Bean
    @Conditional(CustomCondition.class)
    public CustomService customService() {
        return new CustomService();
    }
}
```

## Combining Conditions

### Multiple Conditions

```java
@Configuration
public class CombinedConfig {
    
    @Bean
    @ConditionalOnProperty(name = "feature.enabled", havingValue = "true")
    @ConditionalOnClass(SomeClass.class)
    @ConditionalOnMissingBean(SomeService.class)
    public SomeService someService() {
        return new SomeService();
    }
}
```

## Best Practices

1. **Use Appropriate Conditions**: Choose the right conditional annotation
2. **Provide Defaults**: Use matchIfMissing for sensible defaults
3. **Document Conditions**: Document when beans are created
4. **Test Conditions**: Test with and without conditions
5. **Keep Simple**: Keep conditional logic simple

## Summary

Spring Boot Conditional Beans:
- Create beans conditionally
- Support various conditions
- Enable flexible configuration
- Essential for auto-configuration


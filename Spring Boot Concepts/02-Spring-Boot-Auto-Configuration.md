# Spring Boot Auto Configuration

## What is Auto Configuration?

Auto Configuration is one of the most powerful features of Spring Boot. It automatically configures your Spring application based on the dependencies you have added in the classpath.

## How Auto Configuration Works

### 1. **Conditional Configuration**
Spring Boot uses conditional annotations to determine what to configure:

```java
@Configuration
@ConditionalOnClass(DataSource.class)
@ConditionalOnProperty(name = "spring.datasource.url")
public class DataSourceAutoConfiguration {
    // Auto-configuration logic
}
```

### 2. **Auto Configuration Classes**
Located in `spring-boot-autoconfigure.jar` under `META-INF/spring.factories`:

```properties
org.springframework.boot.autoconfigure.EnableAutoConfiguration=\
org.springframework.boot.autoconfigure.jdbc.DataSourceAutoConfiguration,\
org.springframework.boot.autoconfigure.web.servlet.DispatcherServletAutoConfiguration,\
...
```

## Key Conditional Annotations

### @ConditionalOnClass
Configures bean only if specified class is present:

```java
@ConditionalOnClass(DataSource.class)
public class DataSourceAutoConfiguration {
    // Configuration
}
```

### @ConditionalOnMissingClass
Configures bean only if specified class is NOT present:

```java
@ConditionalOnMissingClass("com.example.CustomClass")
public class DefaultConfiguration {
    // Configuration
}
```

### @ConditionalOnBean
Configures bean only if another bean exists:

```java
@ConditionalOnBean(DataSource.class)
public class JdbcTemplateConfiguration {
    // Configuration
}
```

### @ConditionalOnMissingBean
Configures bean only if another bean does NOT exist:

```java
@ConditionalOnMissingBean(JdbcTemplate.class)
public class JdbcTemplateAutoConfiguration {
    @Bean
    public JdbcTemplate jdbcTemplate(DataSource dataSource) {
        return new JdbcTemplate(dataSource);
    }
}
```

### @ConditionalOnProperty
Configures bean based on property value:

```java
@ConditionalOnProperty(
    name = "spring.datasource.type",
    havingValue = "com.zaxxer.hikari.HikariDataSource"
)
public class HikariDataSourceConfiguration {
    // Configuration
}
```

### @ConditionalOnWebApplication
Configures only in web application context:

```java
@ConditionalOnWebApplication
public class WebMvcAutoConfiguration {
    // Configuration
}
```

### @ConditionalOnNotWebApplication
Configures only in non-web application context:

```java
@ConditionalOnNotWebApplication
public class BatchAutoConfiguration {
    // Configuration
}
```

## Common Auto Configurations

### 1. **DataSource Auto Configuration**
Automatically configures DataSource if:
- Spring JDBC is on classpath
- Database driver is present
- Connection properties are provided

```properties
spring.datasource.url=jdbc:mysql://localhost:3306/mydb
spring.datasource.username=root
spring.datasource.password=password
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
```

### 2. **JPA/Hibernate Auto Configuration**
Automatically configures:
- EntityManagerFactory
- TransactionManager
- Hibernate properties

```properties
spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.MySQLDialect
```

### 3. **Web MVC Auto Configuration**
Automatically configures:
- DispatcherServlet
- ViewResolvers
- MessageConverters
- ExceptionHandlers

### 4. **Jackson Auto Configuration**
Automatically configures JSON serialization/deserialization

### 5. **Security Auto Configuration**
Automatically configures basic security if Spring Security is on classpath

## Disabling Auto Configuration

### Method 1: Exclude Specific Classes

```java
@SpringBootApplication(exclude = {
    DataSourceAutoConfiguration.class,
    JdbcTemplateAutoConfiguration.class
})
public class Application {
    // ...
}
```

### Method 2: Exclude by Name

```java
@SpringBootApplication(excludeName = {
    "org.springframework.boot.autoconfigure.jdbc.DataSourceAutoConfiguration"
})
public class Application {
    // ...
}
```

### Method 3: Using Properties

```properties
spring.autoconfigure.exclude=org.springframework.boot.autoconfigure.jdbc.DataSourceAutoConfiguration
```

## Creating Custom Auto Configuration

### Step 1: Create Configuration Class

```java
@Configuration
@ConditionalOnClass(MyService.class)
@ConditionalOnProperty(name = "my.service.enabled", havingValue = "true", matchIfMissing = true)
public class MyServiceAutoConfiguration {
    
    @Bean
    @ConditionalOnMissingBean
    public MyService myService() {
        return new MyService();
    }
}
```

### Step 2: Register in spring.factories

Create `META-INF/spring.factories`:

```properties
org.springframework.boot.autoconfigure.EnableAutoConfiguration=\
com.example.MyServiceAutoConfiguration
```

### Step 3: Add Conditional Dependencies

```java
@Configuration
@AutoConfigureAfter(DataSourceAutoConfiguration.class)
@ConditionalOnBean(DataSource.class)
public class MyServiceAutoConfiguration {
    // Configuration
}
```

## Auto Configuration Report

### Enable Debug Mode

Add to `application.properties`:

```properties
debug=true
```

This will show:
- Positive matches (auto-configurations that were applied)
- Negative matches (auto-configurations that were not applied)
- Exclusions

### Using Actuator

```properties
management.endpoints.web.exposure.include=*
```

Access: `http://localhost:8080/actuator/conditions`

## Auto Configuration Order

Spring Boot processes auto-configuration in a specific order:

1. **@AutoConfigureBefore**: Configure before specified classes
2. **@AutoConfigureAfter**: Configure after specified classes
3. **@AutoConfigureOrder**: Set explicit order

```java
@Configuration
@AutoConfigureAfter(DataSourceAutoConfiguration.class)
@AutoConfigureOrder(Ordered.HIGHEST_PRECEDENCE)
public class MyAutoConfiguration {
    // Configuration
}
```

## Best Practices

1. **Use Conditional Annotations**: Always use appropriate conditional annotations
2. **Check for Existing Beans**: Use `@ConditionalOnMissingBean` to avoid conflicts
3. **Provide Sensible Defaults**: Set reasonable default values
4. **Document Properties**: Document all configuration properties
5. **Test Auto Configuration**: Test with and without dependencies

## Common Auto Configuration Classes

| Auto Configuration | Purpose |
|-------------------|---------|
| `DataSourceAutoConfiguration` | Database connection |
| `JpaRepositoriesAutoConfiguration` | JPA repositories |
| `HibernateJpaAutoConfiguration` | Hibernate settings |
| `DispatcherServletAutoConfiguration` | Web MVC |
| `JacksonAutoConfiguration` | JSON processing |
| `SecurityAutoConfiguration` | Spring Security |
| `CacheAutoConfiguration` | Caching support |
| `MailSenderAutoConfiguration` | Email support |

## Troubleshooting

### Issue: Auto Configuration Not Working

**Solutions:**
1. Check if dependency is in classpath
2. Verify `@SpringBootApplication` is used
3. Check for exclusions
4. Enable debug mode to see conditions

### Issue: Multiple Auto Configurations Conflict

**Solutions:**
1. Use `@ConditionalOnMissingBean`
2. Exclude conflicting auto-configurations
3. Use `@Primary` to set preferred bean

## Summary

Auto Configuration:
- Reduces boilerplate code
- Provides sensible defaults
- Can be customized or disabled
- Works based on classpath and properties
- Makes Spring Boot applications easy to set up


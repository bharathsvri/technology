# Spring Boot Application Properties

## What are Application Properties?

Application properties in Spring Boot are configuration files used to externalize application configuration. They allow you to configure your application without changing code.

## Property File Formats

Spring Boot supports multiple property file formats:

1. **application.properties** (most common)
2. **application.yml** (YAML format)
3. **application.yaml** (YAML format)

## Property File Locations

Spring Boot looks for property files in the following order (higher priority overrides lower):

1. `/config` subdirectory of current directory
2. Current directory
3. `classpath:/config` package
4. `classpath:` root

## Basic Property Syntax

### Properties Format

```properties
# application.properties
server.port=8080
spring.application.name=myapp
spring.datasource.url=jdbc:mysql://localhost:3306/mydb
spring.datasource.username=root
spring.datasource.password=password
```

### YAML Format

```yaml
# application.yml
server:
  port: 8080
spring:
  application:
    name: myapp
  datasource:
    url: jdbc:mysql://localhost:3306/mydb
    username: root
    password: password
```

## Common Application Properties

### Server Configuration

```properties
# Server Port
server.port=8080

# Server Address
server.address=0.0.0.0

# Context Path
server.servlet.context-path=/api

# Session Timeout
server.servlet.session.timeout=30m

# Error Pages
server.error.whitelabel.enabled=false
server.error.include-message=always
```

### Spring Application

```properties
# Application Name
spring.application.name=myapp

# Display Banner
spring.main.banner-mode=console

# Web Application Type
spring.main.web-application-type=servlet
```

### Database Configuration

```properties
# H2 Database (In-Memory)
spring.datasource.url=jdbc:h2:mem:testdb
spring.datasource.driver-class-name=org.h2.Driver
spring.datasource.username=sa
spring.datasource.password=

# MySQL Database
spring.datasource.url=jdbc:mysql://localhost:3306/mydb
spring.datasource.username=root
spring.datasource.password=password
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver

# Connection Pool (HikariCP)
spring.datasource.hikari.maximum-pool-size=10
spring.datasource.hikari.minimum-idle=5
spring.datasource.hikari.connection-timeout=30000
```

### JPA/Hibernate Configuration

```properties
# JPA Properties
spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
spring.jpa.properties.hibernate.format_sql=true
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.MySQLDialect

# Hibernate Naming Strategy
spring.jpa.hibernate.naming.physical-strategy=org.hibernate.boot.model.naming.PhysicalNamingStrategyStandardImpl
spring.jpa.hibernate.naming.implicit-strategy=org.hibernate.boot.model.naming.ImplicitNamingStrategyLegacyJpaImpl
```

### Logging Configuration

```properties
# Logging Level
logging.level.root=INFO
logging.level.com.example=DEBUG
logging.level.org.springframework.web=DEBUG

# Log File
logging.file.name=application.log
logging.file.path=/var/log

# Log Pattern
logging.pattern.console=%d{yyyy-MM-dd HH:mm:ss} - %msg%n
logging.pattern.file=%d{yyyy-MM-dd HH:mm:ss} [%thread] %-5level %logger{36} - %msg%n
```

### Security Configuration

```properties
# Security
spring.security.user.name=admin
spring.security.user.password=admin123
spring.security.user.roles=ADMIN,USER
```

### Actuator Configuration

```properties
# Actuator Endpoints
management.endpoints.web.exposure.include=health,info,metrics
management.endpoint.health.show-details=always
management.metrics.export.prometheus.enabled=true
```

### Email Configuration

```properties
# SMTP Configuration
spring.mail.host=smtp.gmail.com
spring.mail.port=587
spring.mail.username=your-email@gmail.com
spring.mail.password=your-password
spring.mail.properties.mail.smtp.auth=true
spring.mail.properties.mail.smtp.starttls.enable=true
```

### Cache Configuration

```properties
# Cache Type
spring.cache.type=simple

# Redis Cache
spring.cache.type=redis
spring.redis.host=localhost
spring.redis.port=6379
```

### File Upload Configuration

```properties
# Multipart File Upload
spring.servlet.multipart.enabled=true
spring.servlet.multipart.max-file-size=10MB
spring.servlet.multipart.max-request-size=10MB
spring.servlet.multipart.file-size-threshold=2KB
```

## Property Placeholders

### Using Properties in Code

```java
@Value("${server.port}")
private int serverPort;

@Value("${app.name:DefaultApp}")
private String appName;
```

### Using @ConfigurationProperties

```java
@ConfigurationProperties(prefix = "app")
public class AppProperties {
    private String name;
    private String version;
    private Database database;
    
    // getters and setters
    
    public static class Database {
        private String url;
        private String username;
        // getters and setters
    }
}
```

```properties
app.name=MyApplication
app.version=1.0.0
app.database.url=jdbc:mysql://localhost:3306/mydb
app.database.username=root
```

## Environment Variables

Spring Boot automatically maps environment variables to properties:

```bash
# Environment Variable
export SERVER_PORT=9090

# Maps to
server.port=9090
```

### Environment Variable Naming

- Uppercase
- Replace dots with underscores
- Replace hyphens with underscores

Example:
- `spring.datasource.url` → `SPRING_DATASOURCE_URL`
- `server.servlet.context-path` → `SERVER_SERVLET_CONTEXT_PATH`

## Command Line Arguments

Properties can be overridden via command line:

```bash
java -jar app.jar --server.port=9090 --spring.profiles.active=prod
```

## Property Profiles

### Profile-Specific Properties

```properties
# application.properties (default)
server.port=8080

# application-dev.properties
server.port=8081
spring.datasource.url=jdbc:h2:mem:devdb

# application-prod.properties
server.port=8080
spring.datasource.url=jdbc:mysql://prod-server:3306/proddb
```

### Activating Profiles

```properties
# In application.properties
spring.profiles.active=dev,prod
```

```bash
# Command line
--spring.profiles.active=prod

# Environment variable
export SPRING_PROFILES_ACTIVE=prod
```

## YAML Configuration

### Basic YAML

```yaml
server:
  port: 8080
  servlet:
    context-path: /api

spring:
  application:
    name: myapp
  datasource:
    url: jdbc:mysql://localhost:3306/mydb
    username: root
    password: password
  jpa:
    hibernate:
      ddl-auto: update
    show-sql: true
```

### YAML with Profiles

```yaml
spring:
  profiles:
    active: dev

---
spring:
  config:
    activate:
      on-profile: dev
server:
  port: 8081

---
spring:
  config:
    activate:
      on-profile: prod
server:
  port: 8080
```

### YAML Lists and Maps

```yaml
app:
  servers:
    - dev.example.com
    - prod.example.com
  config:
    timeout: 30
    retries: 3
```

## Property Validation

### Using @Validated

```java
@ConfigurationProperties(prefix = "app")
@Validated
public class AppProperties {
    @NotNull
    private String name;
    
    @Min(1)
    @Max(100)
    private int port;
    
    // getters and setters
}
```

## External Configuration

### Loading External Properties

```bash
java -jar app.jar --spring.config.location=classpath:/application.properties,file:/path/to/config/
```

### Multiple Property Files

```properties
spring.config.import=optional:file:./config/application.properties
```

## Property Encryption

### Using Jasypt

```xml
<dependency>
    <groupId>com.github.ulisesbocchio</groupId>
    <artifactId>jasypt-spring-boot-starter</artifactId>
    <version>3.0.5</version>
</dependency>
```

```properties
# Encrypted password
spring.datasource.password=ENC(encrypted-password)

# Jasypt password
jasypt.encryptor.password=secret-key
```

## Best Practices

1. **Use YAML for Complex Configurations**: YAML is more readable for nested properties
2. **Use Profiles**: Separate configuration for different environments
3. **Externalize Sensitive Data**: Never commit passwords/secrets
4. **Use Environment Variables**: For deployment-specific values
5. **Validate Properties**: Use `@Validated` and validation annotations
6. **Document Properties**: Document custom properties
7. **Use Type-Safe Configuration**: Prefer `@ConfigurationProperties` over `@Value`

## Property Resolution Order

1. Default properties (Spring Boot defaults)
2. `@PropertySource` annotations
3. Profile-specific properties
4. Environment variables
5. Command line arguments

## Summary

Application Properties:
- Externalize configuration
- Support multiple formats (properties, YAML)
- Support profiles for different environments
- Can be overridden via environment variables or command line
- Enable type-safe configuration with `@ConfigurationProperties`


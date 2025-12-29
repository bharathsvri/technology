# Spring Boot Profiles

## What are Profiles?

Spring Boot Profiles provide a way to segregate parts of your application configuration and make it available only in certain environments. They allow you to have different configurations for development, testing, and production.

## Why Use Profiles?

1. **Environment-Specific Configuration**: Different settings for dev, test, prod
2. **Feature Toggles**: Enable/disable features per environment
3. **Database Configuration**: Different databases per environment
4. **External Service URLs**: Different endpoints per environment
5. **Logging Levels**: Different logging configurations

## Profile Naming Convention

Profiles are typically named:
- `dev` - Development environment
- `test` - Testing environment
- `staging` - Staging environment
- `prod` or `production` - Production environment

## Creating Profile-Specific Properties

### Method 1: Separate Property Files

```
src/main/resources/
├── application.properties
├── application-dev.properties
├── application-test.properties
└── application-prod.properties
```

**application.properties** (default):
```properties
spring.application.name=myapp
server.port=8080
```

**application-dev.properties**:
```properties
spring.datasource.url=jdbc:h2:mem:devdb
spring.jpa.hibernate.ddl-auto=create-drop
spring.jpa.show-sql=true
logging.level.root=DEBUG
```

**application-prod.properties**:
```properties
spring.datasource.url=jdbc:mysql://prod-server:3306/proddb
spring.datasource.username=produser
spring.datasource.password=prodpass
spring.jpa.hibernate.ddl-auto=validate
logging.level.root=WARN
```

### Method 2: YAML with Profiles

**application.yml**:
```yaml
spring:
  application:
    name: myapp
  profiles:
    active: dev

---
spring:
  config:
    activate:
      on-profile: dev
server:
  port: 8081
spring:
  datasource:
    url: jdbc:h2:mem:devdb
  jpa:
    hibernate:
      ddl-auto: create-drop
    show-sql: true
logging:
  level:
    root: DEBUG

---
spring:
  config:
    activate:
      on-profile: prod
server:
  port: 8080
spring:
  datasource:
    url: jdbc:mysql://prod-server:3306/proddb
    username: produser
    password: prodpass
  jpa:
    hibernate:
      ddl-auto: validate
logging:
  level:
    root: WARN
```

## Activating Profiles

### Method 1: application.properties

```properties
spring.profiles.active=dev
```

### Method 2: Command Line

```bash
java -jar app.jar --spring.profiles.active=prod
```

### Method 3: Environment Variable

```bash
export SPRING_PROFILES_ACTIVE=prod
```

### Method 4: Programmatically

```java
@SpringBootApplication
public class Application {
    public static void main(String[] args) {
        SpringApplication app = new SpringApplication(Application.class);
        app.setAdditionalProfiles("prod");
        app.run(args);
    }
}
```

### Method 5: IDE Configuration

In IntelliJ IDEA or Eclipse:
- Run Configuration → VM Options: `-Dspring.profiles.active=dev`

## Multiple Active Profiles

You can activate multiple profiles simultaneously:

```properties
spring.profiles.active=dev,db,logging
```

Profiles are processed in order, with later profiles overriding earlier ones.

## Profile-Specific Beans

### Using @Profile Annotation

```java
@Configuration
public class DatabaseConfig {
    
    @Bean
    @Profile("dev")
    public DataSource devDataSource() {
        return new EmbeddedDatabaseBuilder()
            .setType(EmbeddedDatabaseType.H2)
            .build();
    }
    
    @Bean
    @Profile("prod")
    public DataSource prodDataSource() {
        HikariDataSource dataSource = new HikariDataSource();
        dataSource.setJdbcUrl("jdbc:mysql://prod-server:3306/proddb");
        return dataSource;
    }
}
```

### Multiple Profiles

```java
@Bean
@Profile({"dev", "test"})
public DataSource testDataSource() {
    // Configuration for dev and test
}
```

### Profile Expressions

```java
@Bean
@Profile("!prod")  // Not production
public DataSource nonProdDataSource() {
    // Configuration
}

@Bean
@Profile("dev | test")  // Dev or test
public DataSource devOrTestDataSource() {
    // Configuration
}
```

## Profile-Specific Configuration Classes

```java
@Configuration
@Profile("dev")
public class DevConfiguration {
    
    @Bean
    public MyService myService() {
        return new DevMyService();
    }
}

@Configuration
@Profile("prod")
public class ProdConfiguration {
    
    @Bean
    public MyService myService() {
        return new ProdMyService();
    }
}
```

## Default Profile

### Setting Default Profile

```java
@SpringBootApplication
public class Application {
    public static void main(String[] args) {
        SpringApplication app = new SpringApplication(Application.class);
        app.setDefaultProperties(Collections.singletonMap(
            "spring.profiles.default", "dev"
        ));
        app.run(args);
    }
}
```

## Profile Groups

Spring Boot 2.4+ supports profile groups:

```properties
spring.profiles.group.production=prod,db,mysql
spring.profiles.group.development=dev,h2,debug
```

Activate group:
```bash
--spring.profiles.active=production
```

## Profile-Specific Logging

### logback-spring.xml

```xml
<configuration>
    <springProfile name="dev">
        <appender name="CONSOLE" class="ch.qos.logback.core.ConsoleAppender">
            <encoder>
                <pattern>%d{HH:mm:ss.SSS} [%thread] %-5level %logger{36} - %msg%n</pattern>
            </encoder>
        </appender>
        <root level="DEBUG">
            <appender-ref ref="CONSOLE" />
        </root>
    </springProfile>
    
    <springProfile name="prod">
        <appender name="FILE" class="ch.qos.logback.core.rolling.RollingFileAppender">
            <file>application.log</file>
            <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
                <fileNamePattern>application.%d{yyyy-MM-dd}.log</fileNamePattern>
            </rollingPolicy>
            <encoder>
                <pattern>%d{yyyy-MM-dd HH:mm:ss} [%thread] %-5level %logger{36} - %msg%n</pattern>
            </encoder>
        </appender>
        <root level="WARN">
            <appender-ref ref="FILE" />
        </root>
    </springProfile>
</configuration>
```

## Profile-Specific Application Properties

### Complete Example

**application.properties**:
```properties
# Common properties
spring.application.name=myapp
app.name=My Application
app.version=1.0.0
```

**application-dev.properties**:
```properties
# Development
server.port=8081
spring.datasource.url=jdbc:h2:mem:devdb
spring.jpa.hibernate.ddl-auto=create-drop
spring.jpa.show-sql=true
logging.level.com.example=DEBUG
app.environment=Development
```

**application-prod.properties**:
```properties
# Production
server.port=8080
spring.datasource.url=jdbc:mysql://prod-server:3306/proddb
spring.datasource.username=${DB_USERNAME}
spring.datasource.password=${DB_PASSWORD}
spring.jpa.hibernate.ddl-auto=validate
logging.level.com.example=WARN
app.environment=Production
```

## Checking Active Profiles

### In Code

```java
@Autowired
private Environment environment;

public void checkProfiles() {
    String[] activeProfiles = environment.getActiveProfiles();
    for (String profile : activeProfiles) {
        System.out.println("Active profile: " + profile);
    }
}
```

### Using ApplicationContext

```java
@Autowired
private ApplicationContext context;

public void checkProfiles() {
    String[] profiles = context.getEnvironment().getActiveProfiles();
    // Process profiles
}
```

## Profile-Specific Dependencies

### Maven Profile-Specific Dependencies

```xml
<profiles>
    <profile>
        <id>dev</id>
        <dependencies>
            <dependency>
                <groupId>com.h2database</groupId>
                <artifactId>h2</artifactId>
            </dependency>
        </dependencies>
    </profile>
    <profile>
        <id>prod</id>
        <dependencies>
            <dependency>
                <groupId>mysql</groupId>
                <artifactId>mysql-connector-java</artifactId>
            </dependency>
        </dependencies>
    </profile>
</profiles>
```

## Best Practices

1. **Use Descriptive Names**: Use clear profile names (dev, test, prod)
2. **Keep Common Properties**: Put shared properties in `application.properties`
3. **Environment Variables**: Use environment variables for sensitive data
4. **Document Profiles**: Document what each profile is for
5. **Test Profiles**: Test with different active profiles
6. **Default Profile**: Set a sensible default profile
7. **Profile Groups**: Use profile groups for complex scenarios

## Common Profile Patterns

### Development Profile
- In-memory database (H2)
- Detailed logging
- Auto-create database schema
- Debug mode enabled

### Production Profile
- External database
- Minimal logging
- Validate database schema
- Security enabled
- Performance optimizations

### Test Profile
- Test database
- Mock external services
- Fast startup
- Test-specific configurations

## Profile Inheritance

Properties from `application.properties` are inherited by profile-specific files. Profile-specific properties override common properties.

## Summary

Spring Boot Profiles:
- Allow environment-specific configuration
- Support multiple active profiles
- Enable profile-specific beans
- Support profile groups
- Can be activated via properties, command line, or environment variables
- Essential for managing different deployment environments


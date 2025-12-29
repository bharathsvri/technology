# Spring Boot Starters

## What are Spring Boot Starters?

Spring Boot Starters are dependency descriptors that bring in all the necessary dependencies for a specific feature. They simplify dependency management by providing a one-stop solution for common use cases.

## Benefits of Starters

1. **Simplified Dependency Management**: No need to add multiple dependencies manually
2. **Version Compatibility**: All dependencies are tested and compatible
3. **Consistent Configuration**: Provides consistent setup across projects
4. **Reduced Boilerplate**: Less configuration code needed

## Starter Naming Convention

Spring Boot starters follow the pattern:
- `spring-boot-starter-{name}` for official starters
- `{name}-spring-boot-starter` for third-party starters

## Core Spring Boot Starters

### 1. spring-boot-starter

**Purpose**: Core starter with auto-configuration support and logging

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter</artifactId>
</dependency>
```

**Includes:**
- Spring Core
- Spring Context
- Auto Configuration
- Logging (SLF4J + Logback)

### 2. spring-boot-starter-web

**Purpose**: Building web applications, including RESTful applications

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
```

**Includes:**
- Spring MVC
- Embedded Tomcat
- Jackson (JSON)
- Validation
- Spring Web

**Use Case**: REST APIs, Web applications

### 3. spring-boot-starter-data-jpa

**Purpose**: JPA with Hibernate

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-jpa</artifactId>
</dependency>
```

**Includes:**
- Spring Data JPA
- Hibernate
- Spring ORM
- JDBC

**Use Case**: Database operations with JPA

### 4. spring-boot-starter-jdbc

**Purpose**: JDBC support

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-jdbc</artifactId>
</dependency>
```

**Includes:**
- Spring JDBC
- HikariCP (connection pool)
- JDBC driver (need to add separately)

**Use Case**: Direct JDBC operations

### 5. spring-boot-starter-data-mongodb

**Purpose**: MongoDB support

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-mongodb</artifactId>
</dependency>
```

**Use Case**: MongoDB database operations

### 6. spring-boot-starter-data-redis

**Purpose**: Redis support

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
```

**Use Case**: Caching, session management with Redis

### 7. spring-boot-starter-security

**Purpose**: Spring Security

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-security</artifactId>
</dependency>
```

**Includes:**
- Spring Security
- Authentication
- Authorization
- CSRF protection

**Use Case**: Security, authentication, authorization

### 8. spring-boot-starter-test

**Purpose**: Testing support

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-test</artifactId>
    <scope>test</scope>
</dependency>
```

**Includes:**
- JUnit 5
- Mockito
- AssertJ
- Hamcrest
- Spring Test

**Use Case**: Unit and integration testing

### 9. spring-boot-starter-actuator

**Purpose**: Production-ready features

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
```

**Includes:**
- Health checks
- Metrics
- Monitoring endpoints

**Use Case**: Production monitoring

### 10. spring-boot-starter-mail

**Purpose**: Email support

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-mail</artifactId>
</dependency>
```

**Includes:**
- JavaMailSender
- Spring Email support

**Use Case**: Sending emails

### 11. spring-boot-starter-cache

**Purpose**: Caching abstraction

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-cache</artifactId>
</dependency>
```

**Use Case**: Caching support

### 12. spring-boot-starter-validation

**Purpose**: Bean validation

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-validation</artifactId>
</dependency>
```

**Includes:**
- Hibernate Validator
- Bean Validation API

**Use Case**: Input validation

### 13. spring-boot-starter-thymeleaf

**Purpose**: Thymeleaf template engine

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-thymeleaf</artifactId>
</dependency>
```

**Use Case**: Server-side rendering

### 14. spring-boot-starter-websocket

**Purpose**: WebSocket support

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-websocket</artifactId>
</dependency>
```

**Use Case**: Real-time communication

### 15. spring-boot-starter-batch

**Purpose**: Spring Batch

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-batch</artifactId>
</dependency>
```

**Use Case**: Batch processing

### 16. spring-boot-starter-quartz

**Purpose**: Quartz scheduler

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-quartz</artifactId>
</dependency>
```

**Use Case**: Job scheduling

## Third-Party Starters

### 1. spring-boot-starter-data-elasticsearch

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-elasticsearch</artifactId>
</dependency>
```

### 2. spring-boot-starter-data-cassandra

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-cassandra</artifactId>
</dependency>
```

### 3. spring-boot-starter-amqp

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-amqp</artifactId>
</dependency>
```

## Starter Dependencies

Some starters are "starter dependencies" that only bring in other starters:

### spring-boot-starter-parent

Parent POM providing:
- Default Java version
- UTF-8 encoding
- Dependency management
- Plugin management

```xml
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>3.2.0</version>
    <relativePath/>
</parent>
```

## Creating Custom Starter

### Step 1: Create Starter Module

```xml
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
```

### Step 2: Create Auto Configuration

```java
@Configuration
@ConditionalOnClass(MyService.class)
@EnableConfigurationProperties(MyProperties.class)
public class MyServiceAutoConfiguration {
    
    @Bean
    @ConditionalOnMissingBean
    public MyService myService(MyProperties properties) {
        return new MyService(properties);
    }
}
```

### Step 3: Create Properties Class

```java
@ConfigurationProperties(prefix = "my.service")
public class MyProperties {
    private String name = "default";
    private int timeout = 30;
    // getters and setters
}
```

### Step 4: Register Auto Configuration

Create `META-INF/spring.factories`:

```properties
org.springframework.boot.autoconfigure.EnableAutoConfiguration=\
com.example.MyServiceAutoConfiguration
```

## Starter Best Practices

1. **Use Official Starters**: Prefer official Spring Boot starters
2. **Avoid Duplication**: Don't include dependencies already in starters
3. **Check Transitive Dependencies**: Understand what each starter brings
4. **Version Management**: Use `spring-boot-starter-parent` for version management
5. **Scope Appropriately**: Use correct dependency scopes

## Common Starter Combinations

### Web Application with Database

```xml
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-data-jpa</artifactId>
    </dependency>
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
    </dependency>
</dependencies>
```

### REST API with Security

```xml
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-security</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-validation</artifactId>
    </dependency>
</dependencies>
```

## Starter vs Manual Dependencies

### Using Starter (Recommended)
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
```

### Manual Dependencies (Not Recommended)
```xml
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-webmvc</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-web</artifactId>
</dependency>
<dependency>
    <groupId>org.apache.tomcat.embed</groupId>
    <artifactId>tomcat-embed-core</artifactId>
</dependency>
<!-- ... many more ... -->
```

## Summary

Spring Boot Starters:
- Simplify dependency management
- Ensure version compatibility
- Reduce configuration overhead
- Provide consistent project setup
- Make it easy to add features to Spring Boot applications


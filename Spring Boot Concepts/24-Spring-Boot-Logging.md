# Spring Boot Logging

## Logging in Spring Boot

Spring Boot uses SLF4J (Simple Logging Facade for Java) with Logback as the default logging implementation.

## Logging Configuration

### application.properties

```properties
# Logging Configuration
logging.level.root=INFO
logging.level.com.example=DEBUG
logging.level.org.springframework.web=DEBUG
logging.level.org.hibernate=INFO

# Log File
logging.file.name=application.log
logging.file.path=/var/log

# Log Pattern
logging.pattern.console=%d{yyyy-MM-dd HH:mm:ss} - %msg%n
logging.pattern.file=%d{yyyy-MM-dd HH:mm:ss} [%thread] %-5level %logger{36} - %msg%n
```

## Using Loggers

### In Classes

```java
@Service
public class UserService {
    
    private static final Logger logger = LoggerFactory.getLogger(UserService.class);
    
    public User getUserById(Long id) {
        logger.debug("Getting user with id: {}", id);
        
        try {
            User user = userRepository.findById(id).orElse(null);
            logger.info("User found: {}", user);
            return user;
        } catch (Exception e) {
            logger.error("Error getting user with id: {}", id, e);
            throw e;
        }
    }
}
```

## Logback Configuration

### logback-spring.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
    <include resource="org/springframework/boot/logging/logback/defaults.xml"/>
    
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
                <maxHistory>30</maxHistory>
            </rollingPolicy>
            <encoder>
                <pattern>%d{yyyy-MM-dd HH:mm:ss} [%thread] %-5level %logger{36} - %msg%n</pattern>
            </encoder>
        </appender>
        <root level="INFO">
            <appender-ref ref="FILE" />
        </root>
    </springProfile>
</configuration>
```

## Log Levels

### Available Levels

- **TRACE**: Most detailed
- **DEBUG**: Debug information
- **INFO**: Informational messages
- **WARN**: Warning messages
- **ERROR**: Error messages

### Setting Log Levels

```properties
logging.level.com.example=DEBUG
logging.level.org.springframework=INFO
logging.level.org.hibernate.SQL=DEBUG
```

## Structured Logging

### JSON Logging

```xml
<dependency>
    <groupId>net.logstash.logback</groupId>
    <artifactId>logstash-logback-encoder</artifactId>
</dependency>
```

```xml
<appender name="JSON" class="ch.qos.logback.core.ConsoleAppender">
    <encoder class="net.logstash.logback.encoder.LoggingEventCompositeJsonEncoder">
        <providers>
            <timestamp/>
            <version/>
            <logLevel/>
            <message/>
            <mdc/>
        </providers>
    </encoder>
</appender>
```

## Best Practices

1. **Use Appropriate Levels**: Use correct log levels
2. **Include Context**: Include relevant context in logs
3. **Avoid Sensitive Data**: Don't log passwords or tokens
4. **Use Structured Logging**: Use structured logging for production
5. **Rotate Logs**: Configure log rotation
6. **Monitor Logs**: Set up log monitoring

## Summary

Spring Boot Logging:
- Uses SLF4J with Logback
- Easy to configure
- Supports multiple appenders
- Essential for debugging and monitoring


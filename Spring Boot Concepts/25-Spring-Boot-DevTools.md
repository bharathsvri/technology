# Spring Boot DevTools

## What are DevTools?

Spring Boot DevTools provides development-time features like automatic restart, LiveReload, and property defaults to improve the development experience.

## Setting Up DevTools

### Add Dependency

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-devtools</artifactId>
    <scope>runtime</scope>
    <optional>true</optional>
</dependency>
```

## Automatic Restart

### How It Works

DevTools monitors classpath changes and automatically restarts the application when changes are detected.

### Excluding Resources

```properties
spring.devtools.restart.exclude=static/**,public/**
```

### Disabling Restart

```properties
spring.devtools.restart.enabled=false
```

## LiveReload

### Enable LiveReload

LiveReload automatically refreshes the browser when resources change.

### Configuration

```properties
spring.devtools.livereload.enabled=true
spring.devtools.livereload.port=35729
```

## Property Defaults

### DevTools Property Defaults

DevTools sets sensible defaults for development:
- Disables template caching
- Enables debug logging
- Disables production features

## Remote Debugging

### Remote Application

```properties
spring.devtools.remote.secret=mysecret
```

### Remote Client

```java
public class RemoteClient {
    public static void main(String[] args) {
        SpringApplication.run(RemoteClient.class, args);
    }
}
```

## Best Practices

1. **Use in Development Only**: Don't include in production
2. **Exclude Resources**: Exclude static resources from restart
3. **Use LiveReload**: Enable LiveReload for faster development
4. **Configure IDE**: Configure IDE to compile on save

## Summary

Spring Boot DevTools:
- Automatic restart
- LiveReload support
- Development property defaults
- Improves development experience


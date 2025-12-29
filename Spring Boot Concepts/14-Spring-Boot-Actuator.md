# Spring Boot Actuator

## What is Spring Boot Actuator?

Spring Boot Actuator provides production-ready features to help you monitor and manage your application. It includes health checks, metrics, and other useful endpoints.

## Setting Up Actuator

### Add Dependency

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
```

### Enable Endpoints

```properties
# Expose all endpoints
management.endpoints.web.exposure.include=*

# Expose specific endpoints
management.endpoints.web.exposure.include=health,info,metrics

# Exclude endpoints
management.endpoints.web.exposure.exclude=env,beans
```

## Actuator Endpoints

### Health Endpoint

#### Basic Configuration

```properties
management.endpoint.health.show-details=always
management.health.diskspace.enabled=true
```

#### Access Health Endpoint

```bash
GET /actuator/health
```

Response:
```json
{
  "status": "UP",
  "components": {
    "db": {
      "status": "UP",
      "details": {
        "database": "MySQL",
        "validationQuery": "isValid()"
      }
    },
    "diskSpace": {
      "status": "UP",
      "details": {
        "total": 500000000000,
        "free": 250000000000,
        "threshold": 10485760
      }
    }
  }
}
```

#### Custom Health Indicator

```java
@Component
public class CustomHealthIndicator implements HealthIndicator {
    
    @Override
    public Health health() {
        // Check custom health condition
        boolean isHealthy = checkHealth();
        
        if (isHealthy) {
            return Health.up()
                .withDetail("custom", "Service is running")
                .build();
        } else {
            return Health.down()
                .withDetail("custom", "Service is down")
                .build();
        }
    }
    
    private boolean checkHealth() {
        // Health check logic
        return true;
    }
}
```

### Info Endpoint

#### Basic Configuration

```properties
management.info.env.enabled=true
management.info.java.enabled=true
management.info.os.enabled=true
```

#### Custom Info

```properties
info.app.name=My Application
info.app.version=1.0.0
info.app.description=Spring Boot Application
```

#### Programmatic Info

```java
@Component
public class CustomInfoContributor implements InfoContributor {
    
    @Override
    public void contribute(Info.Builder builder) {
        builder.withDetail("custom", Map.of(
            "author", "John Doe",
            "build-time", LocalDateTime.now().toString()
        ));
    }
}
```

### Metrics Endpoint

#### Access Metrics

```bash
GET /actuator/metrics
```

#### Specific Metric

```bash
GET /actuator/metrics/jvm.memory.used
```

#### Custom Metrics

```java
@Service
public class UserService {
    
    private final Counter userCounter;
    private final Timer userTimer;
    
    public UserService(MeterRegistry meterRegistry) {
        this.userCounter = Counter.builder("users.created")
            .description("Number of users created")
            .register(meterRegistry);
        
        this.userTimer = Timer.builder("user.operation.time")
            .description("Time taken for user operations")
            .register(meterRegistry);
    }
    
    public User createUser(User user) {
        Timer.Sample sample = Timer.start();
        try {
            User savedUser = userRepository.save(user);
            userCounter.increment();
            return savedUser;
        } finally {
            sample.stop(userTimer);
        }
    }
}
```

### Loggers Endpoint

#### View Loggers

```bash
GET /actuator/loggers
```

#### Change Log Level

```bash
POST /actuator/loggers/com.example
Content-Type: application/json

{
  "configuredLevel": "DEBUG"
}
```

### Environment Endpoint

```bash
GET /actuator/env
```

### Beans Endpoint

```bash
GET /actuator/beans
```

### Mappings Endpoint

```bash
GET /actuator/mappings
```

### Thread Dump Endpoint

```bash
GET /actuator/threaddump
```

### Heap Dump Endpoint

```bash
GET /actuator/heapdump
```

## Securing Actuator Endpoints

### Basic Security

```java
@Configuration
public class ActuatorSecurityConfig {
    
    @Bean
    public SecurityFilterChain actuatorSecurityFilterChain(HttpSecurity http) throws Exception {
        http
            .requestMatcher(EndpointRequest.toAnyEndpoint())
            .authorizeHttpRequests(auth -> auth
                .requestMatchers(EndpointRequest.to("health", "info")).permitAll()
                .anyRequest().hasRole("ACTUATOR")
            )
            .httpBasic();
        return http.build();
    }
}
```

## Custom Endpoints

### Creating Custom Endpoint

```java
@Component
@Endpoint(id = "custom")
public class CustomEndpoint {
    
    @ReadOperation
    public Map<String, Object> customInfo() {
        Map<String, Object> info = new HashMap<>();
        info.put("status", "UP");
        info.put("timestamp", LocalDateTime.now());
        return info;
    }
    
    @WriteOperation
    public void customOperation(@Selector String action) {
        // Perform custom operation
    }
}
```

### Web Endpoint

```java
@Component
@WebEndpoint(id = "custom")
public class CustomWebEndpoint {
    
    @ReadOperation
    public Map<String, Object> customInfo() {
        return Map.of("status", "UP");
    }
}
```

## Micrometer Integration

### Prometheus

```xml
<dependency>
    <groupId>io.micrometer</groupId>
    <artifactId>micrometer-registry-prometheus</artifactId>
</dependency>
```

```properties
management.endpoints.web.exposure.include=prometheus
```

### InfluxDB

```xml
<dependency>
    <groupId>io.micrometer</groupId>
    <artifactId>micrometer-registry-influx</artifactId>
</dependency>
```

```properties
management.metrics.export.influx.uri=http://localhost:8086
management.metrics.export.influx.db=mydb
```

## Health Groups

### Creating Health Groups

```properties
management.endpoint.health.group.custom.include=db,diskSpace
management.endpoint.health.group.custom.show-details=always
```

Access:
```bash
GET /actuator/health/custom
```

## Complete Configuration Example

```properties
# Actuator Configuration
management.endpoints.web.exposure.include=health,info,metrics,prometheus
management.endpoint.health.show-details=when-authorized
management.endpoint.health.show-components=always

# Health Indicators
management.health.diskspace.enabled=true
management.health.db.enabled=true

# Metrics
management.metrics.export.prometheus.enabled=true
management.metrics.tags.application=${spring.application.name}

# Info
info.app.name=My Application
info.app.version=1.0.0
info.app.description=Spring Boot Application
```

## Best Practices

1. **Secure Endpoints**: Secure sensitive endpoints
2. **Limit Exposure**: Only expose necessary endpoints
3. **Use Health Groups**: Organize health checks
4. **Custom Metrics**: Add custom business metrics
5. **Monitor Regularly**: Set up monitoring dashboards
6. **Set Up Alerts**: Configure alerts for critical metrics
7. **Document Endpoints**: Document custom endpoints

## Summary

Spring Boot Actuator:
- Provides production-ready monitoring
- Includes health checks and metrics
- Supports custom endpoints
- Integrates with monitoring systems
- Essential for production applications


# Monitoring and Observability

## What is Observability?
Observability includes metrics, logging, and tracing to understand system behavior.

## Spring Boot Actuator
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
```

## Actuator Endpoints
```yaml
management:
  endpoints:
    web:
      exposure:
        include: health,info,metrics,prometheus
  endpoint:
    health:
      show-details: always
```

## Custom Metrics
```java
import io.micrometer.core.instrument.*;

@Service
public class MetricsService {
    
    private final Counter requestCounter;
    private final Timer requestTimer;
    private final Gauge activeUsers;
    
    public MetricsService(MeterRegistry meterRegistry) {
        this.requestCounter = Counter.builder("requests.total")
            .description("Total requests")
            .register(meterRegistry);
        
        this.requestTimer = Timer.builder("request.duration")
            .description("Request duration")
            .register(meterRegistry);
        
        this.activeUsers = Gauge.builder("users.active", this, 
            MetricsService::getActiveUserCount)
            .description("Active users")
            .register(meterRegistry);
    }
    
    public void recordRequest() {
        requestCounter.increment();
    }
    
    public void recordDuration(Duration duration) {
        requestTimer.record(duration);
    }
    
    private double getActiveUserCount() {
        return activeUserService.getCount();
    }
}
```

## Distributed Tracing (Zipkin)
```xml
<dependency>
    <groupId>io.zipkin.reporter2</groupId>
    <artifactId>zipkin-reporter-brave</artifactId>
</dependency>
```

## Logging with Logback
```xml
<configuration>
    <appender name="STDOUT" class="ch.qos.logback.core.ConsoleAppender">
        <encoder>
            <pattern>%d{yyyy-MM-dd HH:mm:ss} [%thread] %-5level %logger{36} - %msg%n</pattern>
        </encoder>
    </appender>
    
    <appender name="FILE" class="ch.qos.logback.core.rolling.RollingFileAppender">
        <file>logs/application.log</file>
        <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
            <fileNamePattern>logs/application.%d{yyyy-MM-dd}.log</fileNamePattern>
            <maxHistory>30</maxHistory>
        </rollingPolicy>
        <encoder>
            <pattern>%d{yyyy-MM-dd HH:mm:ss} [%thread] %-5level %logger{36} - %msg%n</pattern>
        </encoder>
    </appender>
    
    <root level="INFO">
        <appender-ref ref="STDOUT" />
        <appender-ref ref="FILE" />
    </root>
</configuration>
```

## Prometheus Integration
```xml
<dependency>
    <groupId>io.micrometer</groupId>
    <artifactId>micrometer-registry-prometheus</artifactId>
</dependency>
```

## Best Practices
1. Use structured logging
2. Implement health checks
3. Monitor key metrics
4. Set up alerts
5. Use distributed tracing
6. Log at appropriate levels
7. Monitor application performance
8. Track business metrics
9. Use correlation IDs
10. Implement proper log rotation


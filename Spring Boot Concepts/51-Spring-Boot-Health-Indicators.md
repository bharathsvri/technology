# Spring Boot Health Indicators

## Custom Health Indicators

Health indicators allow you to check the health of various components in your application and expose them through the Actuator health endpoint.

## Creating Custom Health Indicator

### Basic Health Indicator

```java
@Component
public class CustomHealthIndicator implements HealthIndicator {
    
    @Override
    public Health health() {
        // Check health condition
        boolean isHealthy = checkHealth();
        
        if (isHealthy) {
            return Health.up()
                .withDetail("custom", "Service is running")
                .withDetail("timestamp", LocalDateTime.now())
                .build();
        } else {
            return Health.down()
                .withDetail("custom", "Service is down")
                .withDetail("error", "Connection failed")
                .build();
        }
    }
    
    private boolean checkHealth() {
        // Health check logic
        return true;
    }
}
```

## Database Health Indicator

### Database Health Check

```java
@Component
public class DatabaseHealthIndicator implements HealthIndicator {
    
    private final DataSource dataSource;
    
    public DatabaseHealthIndicator(DataSource dataSource) {
        this.dataSource = dataSource;
    }
    
    @Override
    public Health health() {
        try (Connection connection = dataSource.getConnection()) {
            if (connection.isValid(1)) {
                return Health.up()
                    .withDetail("database", "Available")
                    .withDetail("validationQuery", "isValid()")
                    .build();
            }
        } catch (SQLException e) {
            return Health.down()
                .withDetail("database", "Unavailable")
                .withException(e)
                .build();
        }
        
        return Health.down().build();
    }
}
```

## External Service Health Indicator

### External API Health Check

```java
@Component
public class ExternalServiceHealthIndicator implements HealthIndicator {
    
    private final RestTemplate restTemplate;
    private final String serviceUrl;
    
    public ExternalServiceHealthIndicator(RestTemplate restTemplate) {
        this.restTemplate = restTemplate;
        this.serviceUrl = "https://api.external-service.com/health";
    }
    
    @Override
    public Health health() {
        try {
            ResponseEntity<String> response = restTemplate.getForEntity(serviceUrl, String.class);
            
            if (response.getStatusCode().is2xxSuccessful()) {
                return Health.up()
                    .withDetail("external-service", "Available")
                    .withDetail("status", response.getStatusCode())
                    .build();
            } else {
                return Health.down()
                    .withDetail("external-service", "Unavailable")
                    .withDetail("status", response.getStatusCode())
                    .build();
            }
        } catch (Exception e) {
            return Health.down()
                .withDetail("external-service", "Unavailable")
                .withException(e)
                .build();
        }
    }
}
```

## Disk Space Health Indicator

### Disk Space Check

```java
@Component
public class DiskSpaceHealthIndicator implements HealthIndicator {
    
    private final FileStore fileStore;
    private final long thresholdBytes;
    
    public DiskSpaceHealthIndicator() {
        this.fileStore = FileSystems.getDefault().getFileStores().iterator().next();
        this.thresholdBytes = 10 * 1024 * 1024; // 10 MB
    }
    
    @Override
    public Health health() {
        try {
            long totalSpace = fileStore.getTotalSpace();
            long freeSpace = fileStore.getUsableSpace();
            long usedSpace = totalSpace - freeSpace;
            
            if (freeSpace < thresholdBytes) {
                return Health.down()
                    .withDetail("disk-space", "Insufficient")
                    .withDetail("free", formatBytes(freeSpace))
                    .withDetail("total", formatBytes(totalSpace))
                    .build();
            }
            
            return Health.up()
                .withDetail("disk-space", "Sufficient")
                .withDetail("free", formatBytes(freeSpace))
                .withDetail("total", formatBytes(totalSpace))
                .withDetail("used", formatBytes(usedSpace))
                .build();
        } catch (IOException e) {
            return Health.down()
                .withException(e)
                .build();
        }
    }
    
    private String formatBytes(long bytes) {
        return String.format("%.2f GB", bytes / (1024.0 * 1024.0 * 1024.0));
    }
}
```

## Health Groups

### Custom Health Groups

```properties
management.endpoint.health.group.custom.include=db,diskSpace,custom
management.endpoint.health.group.custom.show-details=always
```

### Accessing Health Groups

```bash
GET /actuator/health/custom
```

## Conditional Health Indicators

### Conditional Indicator

```java
@Component
@ConditionalOnProperty(name = "health.custom.enabled", havingValue = "true")
public class ConditionalHealthIndicator implements HealthIndicator {
    
    @Override
    public Health health() {
        return Health.up().withDetail("custom", "Enabled").build();
    }
}
```

## Best Practices

1. **Check Critical Components**: Monitor critical components
2. **Provide Details**: Include useful details in health status
3. **Handle Exceptions**: Properly handle exceptions
4. **Use Groups**: Organize health checks into groups
5. **Monitor Performance**: Keep health checks fast

## Summary

Spring Boot Health Indicators:
- Monitor application components
- Custom health checks
- Health groups
- Essential for production monitoring


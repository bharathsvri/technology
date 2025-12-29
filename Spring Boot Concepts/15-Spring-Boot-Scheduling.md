# Spring Boot Scheduling

## What is Scheduling?

Scheduling allows you to run tasks automatically at specified times or intervals. Spring Boot provides easy-to-use scheduling support through the `@Scheduled` annotation.

## Enabling Scheduling

### Enable Scheduling

```java
@SpringBootApplication
@EnableScheduling
public class Application {
    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}
```

## @Scheduled Annotation

### Fixed Rate

Runs at fixed intervals regardless of execution time:

```java
@Component
public class ScheduledTasks {
    
    @Scheduled(fixedRate = 5000) // Every 5 seconds
    public void executeTask() {
        System.out.println("Task executed at: " + LocalDateTime.now());
    }
}
```

### Fixed Delay

Runs with fixed delay after previous execution completes:

```java
@Scheduled(fixedDelay = 5000) // 5 seconds after previous completion
public void executeTask() {
    // Task execution
}
```

### Initial Delay

Delays first execution:

```java
@Scheduled(fixedRate = 5000, initialDelay = 10000) // Start after 10 seconds
public void executeTask() {
    // Task execution
}
```

### Cron Expression

Uses cron expressions for complex scheduling:

```java
@Scheduled(cron = "0 0 12 * * ?") // Every day at 12:00 PM
public void executeTask() {
    // Task execution
}

@Scheduled(cron = "0 0 0 * * ?") // Every day at midnight
public void dailyTask() {
    // Daily task
}

@Scheduled(cron = "0 0/15 * * * ?") // Every 15 minutes
public void frequentTask() {
    // Frequent task
}
```

## Cron Expression Format

```
┌───────────── second (0-59)
│ ┌───────────── minute (0-59)
│ │ ┌───────────── hour (0-23)
│ │ │ ┌───────────── day of month (1-31)
│ │ │ │ ┌───────────── month (1-12)
│ │ │ │ │ ┌───────────── day of week (0-7) (0 or 7 is Sunday)
│ │ │ │ │ │
* * * * * *
```

### Cron Examples

```java
@Scheduled(cron = "0 0 0 * * ?")        // Daily at midnight
@Scheduled(cron = "0 0 12 * * ?")       // Daily at noon
@Scheduled(cron = "0 0 0 1 * ?")        // First day of month at midnight
@Scheduled(cron = "0 0 0 ? * MON")      // Every Monday at midnight
@Scheduled(cron = "0 0 0 ? * MON-FRI")  // Weekdays at midnight
@Scheduled(cron = "0 0/30 * * * ?")     // Every 30 minutes
@Scheduled(cron = "0 */5 * * * ?")      // Every 5 minutes
@Scheduled(cron = "0 0 9-17 * * MON-FRI") // Every hour 9 AM to 5 PM on weekdays
```

## Task Scheduling Configuration

### Custom Task Scheduler

```java
@Configuration
@EnableScheduling
public class SchedulingConfig implements SchedulingConfigurer {
    
    @Override
    public void configureTasks(ScheduledTaskRegistrar taskRegistrar) {
        taskRegistrar.setScheduler(taskScheduler());
    }
    
    @Bean(destroyMethod = "shutdown")
    public Executor taskScheduler() {
        return Executors.newScheduledThreadPool(10);
    }
}
```

### Thread Pool Configuration

```java
@Configuration
@EnableScheduling
public class SchedulingConfig {
    
    @Bean
    public TaskScheduler taskScheduler() {
        ThreadPoolTaskScheduler scheduler = new ThreadPoolTaskScheduler();
        scheduler.setPoolSize(10);
        scheduler.setThreadNamePrefix("scheduled-task-");
        scheduler.setWaitForTasksToCompleteOnShutdown(true);
        scheduler.setAwaitTerminationSeconds(60);
        return scheduler;
    }
}
```

## Conditional Scheduling

### Using @ConditionalOnProperty

```java
@Component
@ConditionalOnProperty(name = "scheduling.enabled", havingValue = "true", matchIfMissing = true)
public class ScheduledTasks {
    
    @Scheduled(fixedRate = 5000)
    public void executeTask() {
        // Only runs if scheduling.enabled=true
    }
}
```

## Dynamic Scheduling

### Programmatic Scheduling

```java
@Component
public class DynamicScheduler {
    
    private final TaskScheduler taskScheduler;
    private ScheduledFuture<?> scheduledTask;
    
    public DynamicScheduler(TaskScheduler taskScheduler) {
        this.taskScheduler = taskScheduler;
    }
    
    public void startScheduledTask() {
        scheduledTask = taskScheduler.scheduleAtFixedRate(
            this::executeTask,
            Duration.ofSeconds(5)
        );
    }
    
    public void stopScheduledTask() {
        if (scheduledTask != null) {
            scheduledTask.cancel(false);
        }
    }
    
    private void executeTask() {
        System.out.println("Dynamic task executed");
    }
}
```

## Complete Example

### Scheduled Service

```java
@Service
public class ScheduledTasks {
    
    private static final Logger logger = LoggerFactory.getLogger(ScheduledTasks.class);
    
    private final UserService userService;
    private final EmailService emailService;
    
    public ScheduledTasks(UserService userService, EmailService emailService) {
        this.userService = userService;
        this.emailService = emailService;
    }
    
    // Run every 5 minutes
    @Scheduled(fixedRate = 300000)
    public void cleanupInactiveUsers() {
        logger.info("Cleaning up inactive users");
        userService.deleteInactiveUsers();
    }
    
    // Run every day at 2 AM
    @Scheduled(cron = "0 0 2 * * ?")
    public void sendDailyReports() {
        logger.info("Sending daily reports");
        emailService.sendDailyReports();
    }
    
    // Run every hour
    @Scheduled(cron = "0 0 * * * ?")
    public void processPendingOrders() {
        logger.info("Processing pending orders");
        orderService.processPendingOrders();
    }
    
    // Run every Monday at 9 AM
    @Scheduled(cron = "0 0 9 ? * MON")
    public void weeklyReport() {
        logger.info("Generating weekly report");
        reportService.generateWeeklyReport();
    }
}
```

## Error Handling

### Handling Exceptions

```java
@Component
public class ScheduledTasks {
    
    @Scheduled(fixedRate = 5000)
    public void executeTask() {
        try {
            // Task execution
        } catch (Exception e) {
            logger.error("Error in scheduled task", e);
            // Handle error
        }
    }
}
```

### Using @Async with Scheduling

```java
@Configuration
@EnableScheduling
@EnableAsync
public class SchedulingConfig {
}

@Component
public class ScheduledTasks {
    
    @Async
    @Scheduled(fixedRate = 5000)
    public void executeAsyncTask() {
        // Runs asynchronously
    }
}
```

## Monitoring Scheduled Tasks

### Tracking Task Execution

```java
@Component
public class ScheduledTasks {
    
    private final MeterRegistry meterRegistry;
    private final Counter taskCounter;
    private final Timer taskTimer;
    
    public ScheduledTasks(MeterRegistry meterRegistry) {
        this.meterRegistry = meterRegistry;
        this.taskCounter = Counter.builder("scheduled.tasks.executed")
            .description("Number of scheduled tasks executed")
            .register(meterRegistry);
        this.taskTimer = Timer.builder("scheduled.tasks.duration")
            .description("Duration of scheduled tasks")
            .register(meterRegistry);
    }
    
    @Scheduled(fixedRate = 5000)
    public void executeTask() {
        Timer.Sample sample = Timer.start();
        try {
            // Task execution
            taskCounter.increment();
        } finally {
            sample.stop(taskTimer);
        }
    }
}
```

## Best Practices

1. **Use Appropriate Scheduling**: Choose fixedRate, fixedDelay, or cron based on needs
2. **Handle Exceptions**: Always handle exceptions in scheduled tasks
3. **Avoid Long-Running Tasks**: Keep tasks short or use async execution
4. **Use Thread Pool**: Configure appropriate thread pool size
5. **Monitor Tasks**: Track task execution and performance
6. **Make Tasks Idempotent**: Tasks should be safe to run multiple times
7. **Use Configuration**: Make scheduling configurable via properties
8. **Log Task Execution**: Log task start and completion

## Quartz Scheduler (Alternative)

### Add Dependency

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-quartz</artifactId>
</dependency>
```

### Quartz Job

```java
@Component
public class QuartzJob extends QuartzJobBean {
    
    @Override
    protected void executeInternal(JobExecutionContext context) {
        // Job execution
    }
}
```

### Quartz Configuration

```java
@Configuration
public class QuartzConfig {
    
    @Bean
    public JobDetail jobDetail() {
        return JobBuilder.newJob(QuartzJob.class)
            .withIdentity("myJob")
            .storeDurably()
            .build();
    }
    
    @Bean
    public Trigger trigger() {
        return TriggerBuilder.newTrigger()
            .forJob(jobDetail())
            .withIdentity("myTrigger")
            .withSchedule(CronScheduleBuilder.cronSchedule("0 0 12 * * ?"))
            .build();
    }
}
```

## Summary

Spring Boot Scheduling:
- Easy to use with `@Scheduled` annotation
- Supports fixed rate, fixed delay, and cron expressions
- Configurable thread pool
- Supports dynamic scheduling
- Can be combined with async execution
- Essential for automated tasks


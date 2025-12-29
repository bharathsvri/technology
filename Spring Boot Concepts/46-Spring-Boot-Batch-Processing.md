# Spring Boot Batch Processing

## What is Spring Batch?

Spring Batch is a framework for batch processing that provides reusable functions for processing large volumes of records, including logging/tracing, transaction management, job processing statistics, job restart, skip, and resource management.

## Setting Up Spring Batch

### Add Dependency

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-batch</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.batch</groupId>
    <artifactId>spring-batch-core</artifactId>
</dependency>
```

### Configuration

```properties
# Batch Configuration
spring.batch.job.enabled=false
spring.batch.initialize-schema=always
```

## Job Configuration

### Basic Job

```java
@Configuration
@EnableBatchProcessing
public class BatchConfig {
    
    @Autowired
    private JobBuilderFactory jobBuilderFactory;
    
    @Autowired
    private StepBuilderFactory stepBuilderFactory;
    
    @Bean
    public Job importUserJob(JobCompletionListener listener, Step step1) {
        return jobBuilderFactory.get("importUserJob")
            .incrementer(new RunIdIncrementer())
            .listener(listener)
            .flow(step1)
            .end()
            .build();
    }
    
    @Bean
    public Step step1(ItemReader<User> reader, ItemProcessor<User, User> processor, 
                     ItemWriter<User> writer) {
        return stepBuilderFactory.get("step1")
            .<User, User>chunk(10)
            .reader(reader)
            .processor(processor)
            .writer(writer)
            .build();
    }
}
```

## Item Reader

### Reading from Database

```java
@Bean
public ItemReader<User> reader(DataSource dataSource) {
    JdbcCursorItemReader<User> reader = new JdbcCursorItemReader<>();
    reader.setDataSource(dataSource);
    reader.setSql("SELECT id, name, email FROM users WHERE processed = false");
    reader.setRowMapper(new BeanPropertyRowMapper<>(User.class));
    return reader;
}
```

### Reading from File

```java
@Bean
public FlatFileItemReader<User> reader() {
    FlatFileItemReader<User> reader = new FlatFileItemReader<>();
    reader.setResource(new ClassPathResource("users.csv"));
    reader.setLineMapper(new DefaultLineMapper<User>() {{
        setLineTokenizer(new DelimitedLineTokenizer() {{
            setNames("name", "email", "age");
        }});
        setFieldSetMapper(new BeanWrapperFieldSetMapper<User>() {{
            setTargetType(User.class);
        }});
    }});
    return reader;
}
```

## Item Processor

### Processing Items

```java
@Component
public class UserItemProcessor implements ItemProcessor<User, User> {
    
    private static final Logger log = LoggerFactory.getLogger(UserItemProcessor.class);
    
    @Override
    public User process(User user) throws Exception {
        final String name = user.getName().toUpperCase();
        final String email = user.getEmail().toUpperCase();
        
        final User transformedUser = new User();
        transformedUser.setName(name);
        transformedUser.setEmail(email);
        transformedUser.setAge(user.getAge());
        
        log.info("Converting (" + user + ") into (" + transformedUser + ")");
        
        return transformedUser;
    }
}
```

## Item Writer

### Writing to Database

```java
@Bean
public JdbcBatchItemWriter<User> writer(DataSource dataSource) {
    JdbcBatchItemWriter<User> writer = new JdbcBatchItemWriter<>();
    writer.setItemSqlParameterSourceProvider(new BeanPropertyItemSqlParameterSourceProvider<>());
    writer.setSql("INSERT INTO processed_users (name, email, age) VALUES (:name, :email, :age)");
    writer.setDataSource(dataSource);
    return writer;
}
```

### Writing to File

```java
@Bean
public FlatFileItemWriter<User> writer() {
    FlatFileItemWriter<User> writer = new FlatFileItemWriter<>();
    writer.setResource(new FileSystemResource("output.csv"));
    writer.setLineAggregator(new DelimitedLineAggregator<User>() {{
        setDelimiter(",");
        setFieldExtractor(new BeanWrapperFieldExtractor<User>() {{
            setNames(new String[]{"name", "email", "age"});
        }});
    }});
    return writer;
}
```

## Job Listener

### Job Completion Listener

```java
@Component
public class JobCompletionListener extends JobExecutionListenerSupport {
    
    private static final Logger log = LoggerFactory.getLogger(JobCompletionListener.class);
    
    @Override
    public void afterJob(JobExecution jobExecution) {
        if (jobExecution.getStatus() == BatchStatus.COMPLETED) {
            log.info("Job completed successfully!");
        } else if (jobExecution.getStatus() == BatchStatus.FAILED) {
            log.error("Job failed!");
        }
    }
}
```

## Scheduling Jobs

### Scheduled Job

```java
@Component
public class ScheduledJobLauncher {
    
    @Autowired
    private JobLauncher jobLauncher;
    
    @Autowired
    private Job importUserJob;
    
    @Scheduled(cron = "0 0 2 * * ?") // Run at 2 AM daily
    public void runJob() throws JobParametersInvalidException, 
                               JobExecutionAlreadyRunningException, 
                               JobRestartException, 
                               JobInstanceAlreadyCompleteException {
        JobParameters jobParameters = new JobParametersBuilder()
            .addLong("time", System.currentTimeMillis())
            .toJobParameters();
        
        jobLauncher.run(importUserJob, jobParameters);
    }
}
```

## Error Handling

### Skip and Retry

```java
@Bean
public Step step1(ItemReader<User> reader, ItemProcessor<User, User> processor, 
                 ItemWriter<User> writer) {
    return stepBuilderFactory.get("step1")
        .<User, User>chunk(10)
        .reader(reader)
        .processor(processor)
        .writer(writer)
        .faultTolerant()
        .skipLimit(10)
        .skip(Exception.class)
        .retryLimit(3)
        .retry(SQLException.class)
        .build();
}
```

## Best Practices

1. **Use Chunk Processing**: Process items in chunks
2. **Handle Errors**: Implement skip and retry logic
3. **Monitor Jobs**: Use job listeners for monitoring
4. **Optimize Performance**: Tune chunk size
5. **Test Jobs**: Test batch jobs thoroughly

## Summary

Spring Boot Batch Processing:
- Process large volumes of data
- Supports reading from various sources
- Supports writing to various destinations
- Essential for ETL operations


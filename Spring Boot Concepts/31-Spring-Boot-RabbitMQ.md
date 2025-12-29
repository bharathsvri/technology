# Spring Boot RabbitMQ

## What is RabbitMQ?

RabbitMQ is a message broker that implements the Advanced Message Queuing Protocol (AMQP). It's used for asynchronous messaging between applications.

## Setting Up RabbitMQ

### Add Dependency

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-amqp</artifactId>
</dependency>
```

### Configuration

```properties
# RabbitMQ Configuration
spring.rabbitmq.host=localhost
spring.rabbitmq.port=5672
spring.rabbitmq.username=guest
spring.rabbitmq.password=guest
spring.rabbitmq.virtual-host=/
```

## Basic Configuration

### Queue and Exchange Configuration

```java
@Configuration
public class RabbitMQConfig {
    
    public static final String QUEUE_NAME = "myQueue";
    public static final String EXCHANGE_NAME = "myExchange";
    public static final String ROUTING_KEY = "myRoutingKey";
    
    @Bean
    public Queue queue() {
        return new Queue(QUEUE_NAME, false);
    }
    
    @Bean
    public TopicExchange exchange() {
        return new TopicExchange(EXCHANGE_NAME);
    }
    
    @Bean
    public Binding binding(Queue queue, TopicExchange exchange) {
        return BindingBuilder
            .bind(queue)
            .to(exchange)
            .with(ROUTING_KEY);
    }
}
```

## Sending Messages

### Message Producer

```java
@Service
public class MessageProducer {
    
    private final RabbitTemplate rabbitTemplate;
    
    public MessageProducer(RabbitTemplate rabbitTemplate) {
        this.rabbitTemplate = rabbitTemplate;
    }
    
    public void sendMessage(String message) {
        rabbitTemplate.convertAndSend(RabbitMQConfig.EXCHANGE_NAME, 
                                      RabbitMQConfig.ROUTING_KEY, 
                                      message);
    }
    
    public void sendObject(Object object) {
        rabbitTemplate.convertAndSend(RabbitMQConfig.EXCHANGE_NAME, 
                                      RabbitMQConfig.ROUTING_KEY, 
                                      object);
    }
}
```

## Receiving Messages

### Message Listener

```java
@Component
public class MessageListener {
    
    @RabbitListener(queues = RabbitMQConfig.QUEUE_NAME)
    public void receiveMessage(String message) {
        System.out.println("Received message: " + message);
    }
    
    @RabbitListener(queues = RabbitMQConfig.QUEUE_NAME)
    public void receiveObject(User user) {
        System.out.println("Received user: " + user.getName());
    }
}
```

## Advanced Configuration

### Multiple Queues

```java
@Configuration
public class RabbitMQConfig {
    
    @Bean
    public Queue userQueue() {
        return new Queue("userQueue", false);
    }
    
    @Bean
    public Queue orderQueue() {
        return new Queue("orderQueue", false);
    }
    
    @Bean
    public TopicExchange exchange() {
        return new TopicExchange("myExchange");
    }
    
    @Bean
    public Binding userBinding(Queue userQueue, TopicExchange exchange) {
        return BindingBuilder
            .bind(userQueue)
            .to(exchange)
            .with("user.*");
    }
    
    @Bean
    public Binding orderBinding(Queue orderQueue, TopicExchange exchange) {
        return BindingBuilder
            .bind(orderQueue)
            .to(exchange)
            .with("order.*");
    }
}
```

## Error Handling

### Error Handler

```java
@Configuration
public class RabbitMQConfig {
    
    @Bean
    public MessageConverter jsonMessageConverter() {
        return new Jackson2JsonMessageConverter();
    }
    
    @Bean
    public RabbitListenerErrorHandler errorHandler() {
        return (amqpMessage, message, exception) -> {
            System.err.println("Error processing message: " + exception.getMessage());
            return null;
        };
    }
}
```

### Using Error Handler

```java
@Component
public class MessageListener {
    
    @RabbitListener(queues = "myQueue", errorHandler = "errorHandler")
    public void processMessage(String message) {
        // Processing logic
    }
}
```

## Best Practices

1. **Use Durable Queues**: Use durable queues for important messages
2. **Handle Errors**: Implement proper error handling
3. **Use Acknowledgments**: Use manual acknowledgments for reliability
4. **Monitor Queues**: Monitor queue lengths and processing times
5. **Use Dead Letter Queues**: Use DLQ for failed messages

## Summary

Spring Boot RabbitMQ:
- Message broker for asynchronous messaging
- Supports various exchange types
- Easy to configure and use
- Essential for microservices communication


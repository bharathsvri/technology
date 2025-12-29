# Spring Boot Kafka

## What is Apache Kafka?

Apache Kafka is a distributed event streaming platform capable of handling trillions of events per day. It's used for building real-time data pipelines and streaming applications.

## Setting Up Kafka

### Add Dependency

```xml
<dependency>
    <groupId>org.springframework.kafka</groupId>
    <artifactId>spring-kafka</artifactId>
</dependency>
```

### Configuration

```properties
# Kafka Configuration
spring.kafka.bootstrap-servers=localhost:9092
spring.kafka.consumer.group-id=my-group
spring.kafka.consumer.auto-offset-reset=earliest
spring.kafka.consumer.key-deserializer=org.apache.kafka.common.serialization.StringDeserializer
spring.kafka.consumer.value-deserializer=org.springframework.kafka.support.serializer.JsonDeserializer
spring.kafka.producer.key-serializer=org.apache.kafka.common.serialization.StringSerializer
spring.kafka.producer.value-serializer=org.springframework.kafka.support.serializer.JsonSerializer
```

## Kafka Producer

### Sending Messages

```java
@Service
public class KafkaProducer {
    
    private final KafkaTemplate<String, Object> kafkaTemplate;
    
    public KafkaProducer(KafkaTemplate<String, Object> kafkaTemplate) {
        this.kafkaTemplate = kafkaTemplate;
    }
    
    public void sendMessage(String topic, String message) {
        kafkaTemplate.send(topic, message);
    }
    
    public void sendMessage(String topic, String key, Object value) {
        kafkaTemplate.send(topic, key, value);
    }
    
    public void sendMessageWithCallback(String topic, Object message) {
        ListenableFuture<SendResult<String, Object>> future = 
            kafkaTemplate.send(topic, message);
        
        future.addCallback(
            result -> System.out.println("Message sent: " + result.getRecordMetadata()),
            failure -> System.err.println("Failed to send: " + failure.getMessage())
        );
    }
}
```

## Kafka Consumer

### Listening to Messages

```java
@Component
public class KafkaConsumer {
    
    @KafkaListener(topics = "my-topic", groupId = "my-group")
    public void consume(String message) {
        System.out.println("Received message: " + message);
    }
    
    @KafkaListener(topics = "user-topic", groupId = "user-group")
    public void consumeUser(User user) {
        System.out.println("Received user: " + user.getName());
    }
    
    @KafkaListener(topics = "order-topic", groupId = "order-group",
                   containerFactory = "kafkaListenerContainerFactory")
    public void consumeOrder(Order order) {
        System.out.println("Received order: " + order.getId());
    }
}
```

## Multiple Topics

### Multiple Topic Listeners

```java
@Component
public class MultiTopicConsumer {
    
    @KafkaListener(topics = {"topic1", "topic2"}, groupId = "multi-group")
    public void consumeFromMultipleTopics(String message) {
        System.out.println("Message from multiple topics: " + message);
    }
    
    @KafkaListener(topicPattern = "user-.*", groupId = "pattern-group")
    public void consumeFromPattern(String message) {
        System.out.println("Message from pattern: " + message);
    }
}
```

## Kafka Configuration

### Producer Configuration

```java
@Configuration
public class KafkaProducerConfig {
    
    @Bean
    public ProducerFactory<String, Object> producerFactory() {
        Map<String, Object> configProps = new HashMap<>();
        configProps.put(ProducerConfig.BOOTSTRAP_SERVERS_CONFIG, "localhost:9092");
        configProps.put(ProducerConfig.KEY_SERIALIZER_CLASS_CONFIG, StringSerializer.class);
        configProps.put(ProducerConfig.VALUE_SERIALIZER_CLASS_CONFIG, JsonSerializer.class);
        return new DefaultKafkaProducerFactory<>(configProps);
    }
    
    @Bean
    public KafkaTemplate<String, Object> kafkaTemplate() {
        return new KafkaTemplate<>(producerFactory());
    }
}
```

### Consumer Configuration

```java
@Configuration
public class KafkaConsumerConfig {
    
    @Bean
    public ConsumerFactory<String, Object> consumerFactory() {
        Map<String, Object> configProps = new HashMap<>();
        configProps.put(ConsumerConfig.BOOTSTRAP_SERVERS_CONFIG, "localhost:9092");
        configProps.put(ConsumerConfig.GROUP_ID_CONFIG, "my-group");
        configProps.put(ConsumerConfig.KEY_DESERIALIZER_CLASS_CONFIG, StringDeserializer.class);
        configProps.put(ConsumerConfig.VALUE_DESERIALIZER_CLASS_CONFIG, JsonDeserializer.class);
        configProps.put(JsonDeserializer.TRUSTED_PACKAGES, "*");
        return new DefaultKafkaConsumerFactory<>(configProps);
    }
    
    @Bean
    public ConcurrentKafkaListenerContainerFactory<String, Object> kafkaListenerContainerFactory() {
        ConcurrentKafkaListenerContainerFactory<String, Object> factory = 
            new ConcurrentKafkaListenerContainerFactory<>();
        factory.setConsumerFactory(consumerFactory());
        return factory;
    }
}
```

## Error Handling

### Error Handler

```java
@Configuration
public class KafkaConfig {
    
    @Bean
    public ConcurrentKafkaListenerContainerFactory<String, Object> kafkaListenerContainerFactory() {
        ConcurrentKafkaListenerContainerFactory<String, Object> factory = 
            new ConcurrentKafkaListenerContainerFactory<>();
        factory.setConsumerFactory(consumerFactory());
        factory.setCommonErrorHandler(new DefaultErrorHandler());
        return factory;
    }
}
```

### Custom Error Handler

```java
@Component
public class CustomErrorHandler implements ConsumerAwareErrorHandler {
    
    @Override
    public void handle(Exception thrownException, List<ConsumerRecord<?, ?>> records,
                      Consumer<?, ?> consumer, MessageListenerContainer container) {
        System.err.println("Error in Kafka listener: " + thrownException.getMessage());
        // Custom error handling logic
    }
}
```

## Transaction Support

### Transactional Producer

```java
@Configuration
public class KafkaConfig {
    
    @Bean
    public ProducerFactory<String, Object> producerFactory() {
        Map<String, Object> configProps = new HashMap<>();
        configProps.put(ProducerConfig.TRANSACTIONAL_ID_CONFIG, "my-transactional-id");
        // ... other config
        return new DefaultKafkaProducerFactory<>(configProps);
    }
    
    @Bean
    public KafkaTemplate<String, Object> kafkaTemplate() {
        KafkaTemplate<String, Object> template = new KafkaTemplate<>(producerFactory());
        template.setTransactionIdPrefix("tx-");
        return template;
    }
}
```

### Using Transactions

```java
@Service
@Transactional
public class OrderService {
    
    private final KafkaTemplate<String, Object> kafkaTemplate;
    
    @Transactional
    public void processOrder(Order order) {
        // Process order
        orderRepository.save(order);
        
        // Send to Kafka within transaction
        kafkaTemplate.send("order-topic", order);
    }
}
```

## Best Practices

1. **Use Appropriate Serializers**: Use JSON serializers for complex objects
2. **Handle Errors**: Implement proper error handling
3. **Use Consumer Groups**: Use consumer groups for load balancing
4. **Monitor Performance**: Monitor Kafka performance
5. **Use Transactions**: Use transactions for exactly-once semantics

## Summary

Spring Boot Kafka:
- Distributed event streaming
- High throughput messaging
- Supports transactions
- Essential for event-driven architecture


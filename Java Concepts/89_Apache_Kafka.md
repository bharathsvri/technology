# Apache Kafka

## What is Kafka?
Kafka is a distributed event streaming platform for building real-time data pipelines.

## Producer
```java
import org.apache.kafka.clients.producer.*;

public class KafkaProducerExample {
    public static void main(String[] args) {
        Properties props = new Properties();
        props.put("bootstrap.servers", "localhost:9092");
        props.put("key.serializer", "org.apache.kafka.common.serialization.StringSerializer");
        props.put("value.serializer", "org.apache.kafka.common.serialization.StringSerializer");
        
        Producer<String, String> producer = new KafkaProducer<>(props);
        
        ProducerRecord<String, String> record = 
            new ProducerRecord<>("my-topic", "key", "value");
        
        producer.send(record, (metadata, exception) -> {
            if (exception == null) {
                System.out.println("Sent to: " + metadata.topic() + 
                                 " partition: " + metadata.partition() + 
                                 " offset: " + metadata.offset());
            } else {
                exception.printStackTrace();
            }
        });
        
        producer.close();
    }
}
```

## Consumer
```java
import org.apache.kafka.clients.consumer.*;

public class KafkaConsumerExample {
    public static void main(String[] args) {
        Properties props = new Properties();
        props.put("bootstrap.servers", "localhost:9092");
        props.put("group.id", "my-group");
        props.put("key.deserializer", "org.apache.kafka.common.serialization.StringDeserializer");
        props.put("value.deserializer", "org.apache.kafka.common.serialization.StringDeserializer");
        
        Consumer<String, String> consumer = new KafkaConsumer<>(props);
        consumer.subscribe(Collections.singletonList("my-topic"));
        
        while (true) {
            ConsumerRecords<String, String> records = consumer.poll(Duration.ofMillis(100));
            for (ConsumerRecord<String, String> record : records) {
                System.out.println("Received: " + record.value());
            }
        }
    }
}
```

## Spring Kafka
```java
import org.springframework.kafka.annotation.*;
import org.springframework.kafka.core.*;

@Service
public class KafkaService {
    
    @Autowired
    private KafkaTemplate<String, String> kafkaTemplate;
    
    public void sendMessage(String topic, String message) {
        kafkaTemplate.send(topic, message);
    }
    
    @KafkaListener(topics = "my-topic", groupId = "my-group")
    public void consume(String message) {
        System.out.println("Consumed: " + message);
    }
}
```

## Best Practices
1. Use appropriate partition keys
2. Handle producer errors
3. Use consumer groups properly
4. Implement idempotent consumers
5. Monitor lag
6. Use compression
7. Configure retention policies
8. Handle rebalancing


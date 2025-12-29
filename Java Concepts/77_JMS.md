# JMS (Java Message Service)

## What is JMS?
JMS is an API for sending messages between applications or components.

## Point-to-Point (Queue)

### Producer
```java
import javax.jms.*;

public class QueueProducer {
    public static void main(String[] args) {
        ConnectionFactory connectionFactory = 
            new ActiveMQConnectionFactory("tcp://localhost:61616");
        
        try (Connection connection = connectionFactory.createConnection();
             Session session = connection.createSession(false, 
                 Session.AUTO_ACKNOWLEDGE);
             Queue queue = session.createQueue("myQueue");
             MessageProducer producer = session.createProducer(queue)) {
            
            connection.start();
            
            TextMessage message = session.createTextMessage("Hello JMS");
            producer.send(message);
            System.out.println("Message sent");
        } catch (JMSException e) {
            e.printStackTrace();
        }
    }
}
```

### Consumer
```java
import javax.jms.*;

public class QueueConsumer {
    public static void main(String[] args) {
        ConnectionFactory connectionFactory = 
            new ActiveMQConnectionFactory("tcp://localhost:61616");
        
        try (Connection connection = connectionFactory.createConnection();
             Session session = connection.createSession(false, 
                 Session.AUTO_ACKNOWLEDGE);
             Queue queue = session.createQueue("myQueue");
             MessageConsumer consumer = session.createConsumer(queue)) {
            
            connection.start();
            
            Message message = consumer.receive();
            if (message instanceof TextMessage) {
                TextMessage textMessage = (TextMessage) message;
                System.out.println("Received: " + textMessage.getText());
            }
        } catch (JMSException e) {
            e.printStackTrace();
        }
    }
}
```

## Publish-Subscribe (Topic)

### Publisher
```java
import javax.jms.*;

public class TopicPublisher {
    public static void main(String[] args) {
        ConnectionFactory connectionFactory = 
            new ActiveMQConnectionFactory("tcp://localhost:61616");
        
        try (Connection connection = connectionFactory.createConnection();
             Session session = connection.createSession(false, 
                 Session.AUTO_ACKNOWLEDGE);
             Topic topic = session.createTopic("myTopic");
             MessageProducer producer = session.createProducer(topic)) {
            
            connection.start();
            
            TextMessage message = session.createTextMessage("News update");
            producer.send(message);
        } catch (JMSException e) {
            e.printStackTrace();
        }
    }
}
```

### Subscriber
```java
import javax.jms.*;

public class TopicSubscriber implements MessageListener {
    public static void main(String[] args) {
        ConnectionFactory connectionFactory = 
            new ActiveMQConnectionFactory("tcp://localhost:61616");
        
        try (Connection connection = connectionFactory.createConnection();
             Session session = connection.createSession(false, 
                 Session.AUTO_ACKNOWLEDGE);
             Topic topic = session.createTopic("myTopic");
             MessageConsumer consumer = session.createConsumer(topic)) {
            
            consumer.setMessageListener(new TopicSubscriber());
            connection.start();
            
            // Keep running
            Thread.sleep(60000);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
    
    @Override
    public void onMessage(Message message) {
        try {
            if (message instanceof TextMessage) {
                TextMessage textMessage = (TextMessage) message;
                System.out.println("Received: " + textMessage.getText());
            }
        } catch (JMSException e) {
            e.printStackTrace();
        }
    }
}
```

## Message Types
```java
// Text message
TextMessage textMsg = session.createTextMessage("Hello");

// Object message
ObjectMessage objMsg = session.createObjectMessage(myObject);

// Map message
MapMessage mapMsg = session.createMapMessage();
mapMsg.setString("key", "value");

// Bytes message
BytesMessage bytesMsg = session.createBytesMessage();
bytesMsg.writeBytes(data);

// Stream message
StreamMessage streamMsg = session.createStreamMessage();
streamMsg.writeString("data");
```

## Best Practices
1. Use connection pooling
2. Handle exceptions properly
3. Use appropriate acknowledgment mode
4. Implement message selectors
5. Use transactions when needed
6. Handle connection failures
7. Monitor message queues
8. Use durable subscriptions for topics


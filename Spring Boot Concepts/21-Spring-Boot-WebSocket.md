# Spring Boot WebSocket

## What is WebSocket?

WebSocket is a communication protocol that provides full-duplex communication channels over a single TCP connection. It enables real-time, bidirectional communication between client and server.

## Setting Up WebSocket

### Add Dependency

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-websocket</artifactId>
</dependency>
```

## WebSocket Configuration

### Enable WebSocket

```java
@Configuration
@EnableWebSocketMessageBroker
public class WebSocketConfig implements WebSocketMessageBrokerConfigurer {
    
    @Override
    public void configureMessageBroker(MessageBrokerRegistry config) {
        config.enableSimpleBroker("/topic", "/queue");
        config.setApplicationDestinationPrefixes("/app");
    }
    
    @Override
    public void registerStompEndpoints(StompEndpointRegistry registry) {
        registry.addEndpoint("/ws").withSockJS();
    }
}
```

## STOMP Messaging

### Message Controller

```java
@Controller
public class ChatController {
    
    @MessageMapping("/chat.sendMessage")
    @SendTo("/topic/public")
    public ChatMessage sendMessage(@Payload ChatMessage chatMessage) {
        return chatMessage;
    }
    
    @MessageMapping("/chat.addUser")
    @SendTo("/topic/public")
    public ChatMessage addUser(@Payload ChatMessage chatMessage,
                               SimpMessageHeaderAccessor headerAccessor) {
        headerAccessor.getSessionAttributes().put("username", chatMessage.getSender());
        return chatMessage;
    }
}
```

### Chat Message Model

```java
public class ChatMessage {
    private MessageType type;
    private String content;
    private String sender;
    
    public enum MessageType {
        CHAT, JOIN, LEAVE
    }
    
    // Constructors, getters, setters
}
```

## Client-Side Implementation

### JavaScript Client

```javascript
const socket = new SockJS('/ws');
const stompClient = Stomp.over(socket);

stompClient.connect({}, function (frame) {
    console.log('Connected: ' + frame);
    
    stompClient.subscribe('/topic/public', function (message) {
        const chatMessage = JSON.parse(message.body);
        displayMessage(chatMessage);
    });
    
    stompClient.send("/app/chat.addUser", {}, 
        JSON.stringify({sender: username, type: 'JOIN'}));
});

function sendMessage() {
    const message = {
        sender: username,
        content: document.getElementById('message').value,
        type: 'CHAT'
    };
    
    stompClient.send("/app/chat.sendMessage", {}, JSON.stringify(message));
}
```

## Security

### WebSocket Security Configuration

```java
@Configuration
@EnableWebSocketSecurity
public class WebSocketSecurityConfig {
    
    @Bean
    public AuthorizationManager<Message<?>> messageAuthorizationManager() {
        MessageMatcherRegistry messages = AuthorizationManagerMessageMatcherRegistry.builder()
            .simpDestMatchers("/topic/public").permitAll()
            .simpDestMatchers("/topic/private").hasRole("USER")
            .build();
        return messages;
    }
}
```

## Complete Example

### WebSocket Configuration

```java
@Configuration
@EnableWebSocketMessageBroker
public class WebSocketConfig implements WebSocketMessageBrokerConfigurer {
    
    @Override
    public void configureMessageBroker(MessageBrokerRegistry config) {
        config.enableSimpleBroker("/topic");
        config.setApplicationDestinationPrefixes("/app");
    }
    
    @Override
    public void registerStompEndpoints(StompEndpointRegistry registry) {
        registry.addEndpoint("/ws")
            .setAllowedOriginPatterns("*")
            .withSockJS();
    }
}
```

### Notification Controller

```java
@Controller
public class NotificationController {
    
    private final SimpMessagingTemplate messagingTemplate;
    
    public NotificationController(SimpMessagingTemplate messagingTemplate) {
        this.messagingTemplate = messagingTemplate;
    }
    
    @MessageMapping("/notifications")
    @SendTo("/topic/notifications")
    public Notification sendNotification(@Payload Notification notification) {
        return notification;
    }
    
    public void sendNotificationToUser(String username, Notification notification) {
        messagingTemplate.convertAndSendToUser(
            username, "/queue/notifications", notification);
    }
}
```

## Best Practices

1. **Use STOMP**: Use STOMP for structured messaging
2. **Handle Disconnections**: Properly handle client disconnections
3. **Secure Endpoints**: Secure WebSocket endpoints
4. **Error Handling**: Handle errors gracefully
5. **Rate Limiting**: Implement rate limiting
6. **Message Validation**: Validate incoming messages

## Summary

Spring Boot WebSocket:
- Enables real-time communication
- Supports STOMP messaging
- Easy to configure
- Essential for real-time applications


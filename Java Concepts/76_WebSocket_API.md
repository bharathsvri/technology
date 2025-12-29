# WebSocket API

## What is WebSocket?
WebSocket provides full-duplex communication over a single TCP connection.

## Server Endpoint
```java
import javax.websocket.*;
import javax.websocket.server.ServerEndpoint;
import java.io.IOException;
import java.util.Set;
import java.util.concurrent.CopyOnWriteArraySet;

@ServerEndpoint("/chat")
public class ChatEndpoint {
    private static Set<Session> sessions = new CopyOnWriteArraySet<>();
    
    @OnOpen
    public void onOpen(Session session) {
        sessions.add(session);
        System.out.println("Client connected: " + session.getId());
    }
    
    @OnMessage
    public void onMessage(String message, Session session) {
        System.out.println("Received: " + message);
        // Broadcast to all clients
        broadcast(message, session);
    }
    
    @OnClose
    public void onClose(Session session) {
        sessions.remove(session);
        System.out.println("Client disconnected: " + session.getId());
    }
    
    @OnError
    public void onError(Throwable error, Session session) {
        error.printStackTrace();
    }
    
    private void broadcast(String message, Session sender) {
        sessions.forEach(session -> {
            if (session.isOpen() && !session.equals(sender)) {
                try {
                    session.getBasicRemote().sendText(message);
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        });
    }
}
```

## Client Endpoint
```java
import javax.websocket.*;
import java.net.URI;

@ClientEndpoint
public class WebSocketClient {
    private Session session;
    
    @OnOpen
    public void onOpen(Session session) {
        this.session = session;
        System.out.println("Connected to server");
    }
    
    @OnMessage
    public void onMessage(String message) {
        System.out.println("Received: " + message);
    }
    
    @OnClose
    public void onClose() {
        System.out.println("Connection closed");
    }
    
    public void sendMessage(String message) {
        try {
            session.getBasicRemote().sendText(message);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
    
    public static void main(String[] args) {
        try {
            WebSocketContainer container = 
                ContainerProvider.getWebSocketContainer();
            URI uri = new URI("ws://localhost:8080/chat");
            container.connectToServer(WebSocketClient.class, uri);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

## Spring WebSocket
```java
import org.springframework.web.socket.*;
import org.springframework.web.socket.handler.TextWebSocketHandler;

public class ChatHandler extends TextWebSocketHandler {
    @Override
    public void afterConnectionEstablished(WebSocketSession session) {
        // Handle connection
    }
    
    @Override
    protected void handleTextMessage(WebSocketSession session, 
                                    TextMessage message) {
        // Handle message
    }
    
    @Override
    public void afterConnectionClosed(WebSocketSession session, 
                                     CloseStatus status) {
        // Handle disconnection
    }
}
```

## Best Practices
1. Handle connection errors
2. Implement reconnection logic
3. Use heartbeats/ping-pong
4. Limit message size
5. Authenticate connections
6. Handle concurrent connections
7. Use appropriate protocols
8. Monitor connection health


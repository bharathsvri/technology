# Networking

## Socket Programming

### TCP Server
```java
import java.io.*;
import java.net.*;

class TCPServer {
    public static void main(String[] args) {
        try (ServerSocket serverSocket = new ServerSocket(8080)) {
            System.out.println("Server started on port 8080");
            
            while (true) {
                Socket clientSocket = serverSocket.accept();
                System.out.println("Client connected: " + 
                    clientSocket.getInetAddress());
                
                // Handle client in separate thread
                new Thread(() -> {
                    try (BufferedReader in = new BufferedReader(
                            new InputStreamReader(clientSocket.getInputStream()));
                         PrintWriter out = new PrintWriter(
                            clientSocket.getOutputStream(), true)) {
                        
                        String inputLine;
                        while ((inputLine = in.readLine()) != null) {
                            System.out.println("Received: " + inputLine);
                            out.println("Echo: " + inputLine);
                        }
                    } catch (IOException e) {
                        e.printStackTrace();
                    }
                }).start();
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

### TCP Client
```java
import java.io.*;
import java.net.*;

class TCPClient {
    public static void main(String[] args) {
        try (Socket socket = new Socket("localhost", 8080);
             PrintWriter out = new PrintWriter(
                socket.getOutputStream(), true);
             BufferedReader in = new BufferedReader(
                new InputStreamReader(socket.getInputStream()));
             BufferedReader stdIn = new BufferedReader(
                new InputStreamReader(System.in))) {
            
            String userInput;
            while ((userInput = stdIn.readLine()) != null) {
                out.println(userInput);
                String response = in.readLine();
                System.out.println("Server: " + response);
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

### UDP Server
```java
import java.net.*;

class UDPServer {
    public static void main(String[] args) {
        try (DatagramSocket socket = new DatagramSocket(8080)) {
            byte[] buffer = new byte[1024];
            
            while (true) {
                DatagramPacket packet = new DatagramPacket(buffer, buffer.length);
                socket.receive(packet);
                
                String received = new String(packet.getData(), 0, packet.getLength());
                System.out.println("Received: " + received);
                
                // Send response
                String response = "Echo: " + received;
                byte[] responseData = response.getBytes();
                DatagramPacket responsePacket = new DatagramPacket(
                    responseData, responseData.length,
                    packet.getAddress(), packet.getPort());
                socket.send(responsePacket);
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

### UDP Client
```java
import java.net.*;

class UDPClient {
    public static void main(String[] args) {
        try (DatagramSocket socket = new DatagramSocket()) {
            InetAddress address = InetAddress.getByName("localhost");
            String message = "Hello Server";
            byte[] buffer = message.getBytes();
            
            DatagramPacket packet = new DatagramPacket(
                buffer, buffer.length, address, 8080);
            socket.send(packet);
            
            // Receive response
            byte[] responseBuffer = new byte[1024];
            DatagramPacket responsePacket = 
                new DatagramPacket(responseBuffer, responseBuffer.length);
            socket.receive(responsePacket);
            
            String response = new String(
                responsePacket.getData(), 0, responsePacket.getLength());
            System.out.println("Server: " + response);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

## HTTP Client (Java 11+)
```java
import java.net.http.*;
import java.net.URI;

class HttpClientExample {
    public static void main(String[] args) {
        HttpClient client = HttpClient.newHttpClient();
        
        // GET request
        HttpRequest request = HttpRequest.newBuilder()
            .uri(URI.create("https://api.example.com/data"))
            .GET()
            .build();
        
        try {
            HttpResponse<String> response = client.send(
                request, HttpResponse.BodyHandlers.ofString());
            System.out.println("Status: " + response.statusCode());
            System.out.println("Body: " + response.body());
        } catch (Exception e) {
            e.printStackTrace();
        }
        
        // POST request
        HttpRequest postRequest = HttpRequest.newBuilder()
            .uri(URI.create("https://api.example.com/data"))
            .header("Content-Type", "application/json")
            .POST(HttpRequest.BodyPublishers.ofString("{\"key\":\"value\"}"))
            .build();
        
        try {
            HttpResponse<String> response = client.send(
                postRequest, HttpResponse.BodyHandlers.ofString());
            System.out.println(response.body());
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

## URL and URLConnection
```java
import java.net.*;
import java.io.*;

class URLExample {
    public static void main(String[] args) {
        try {
            URL url = new URL("https://www.example.com");
            
            // Get URL components
            System.out.println("Protocol: " + url.getProtocol());
            System.out.println("Host: " + url.getHost());
            System.out.println("Port: " + url.getPort());
            System.out.println("Path: " + url.getPath());
            
            // Read from URL
            URLConnection connection = url.openConnection();
            try (BufferedReader reader = new BufferedReader(
                    new InputStreamReader(connection.getInputStream()))) {
                String line;
                while ((line = reader.readLine()) != null) {
                    System.out.println(line);
                }
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

## NIO Networking (Non-blocking)
```java
import java.nio.channels.*;
import java.nio.ByteBuffer;
import java.net.InetSocketAddress;
import java.util.Iterator;
import java.util.Set;

class NIOServer {
    public static void main(String[] args) {
        try {
            ServerSocketChannel serverChannel = ServerSocketChannel.open();
            serverChannel.bind(new InetSocketAddress(8080));
            serverChannel.configureBlocking(false);
            
            Selector selector = Selector.open();
            serverChannel.register(selector, SelectionKey.OP_ACCEPT);
            
            while (true) {
                selector.select();
                Set<SelectionKey> keys = selector.selectedKeys();
                Iterator<SelectionKey> iterator = keys.iterator();
                
                while (iterator.hasNext()) {
                    SelectionKey key = iterator.next();
                    iterator.remove();
                    
                    if (key.isAcceptable()) {
                        ServerSocketChannel server = (ServerSocketChannel) key.channel();
                        SocketChannel client = server.accept();
                        client.configureBlocking(false);
                        client.register(selector, SelectionKey.OP_READ);
                    } else if (key.isReadable()) {
                        SocketChannel client = (SocketChannel) key.channel();
                        ByteBuffer buffer = ByteBuffer.allocate(1024);
                        client.read(buffer);
                        buffer.flip();
                        // Process data
                    }
                }
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

## Best Practices
1. Always close sockets and streams
2. Handle exceptions properly
3. Use try-with-resources
4. Consider using NIO for high-performance applications
5. Implement proper timeout handling
6. Use thread pools for multiple clients
7. Validate input from network
8. Use HTTPS for secure communication


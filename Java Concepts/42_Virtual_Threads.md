# Virtual Threads (Java 21+)

## What are Virtual Threads?
Virtual threads are lightweight threads managed by the JVM. They allow creating millions of threads without the overhead of platform threads.

## Creating Virtual Threads

### Using Thread.startVirtualThread()
```java
Thread virtualThread = Thread.startVirtualThread(() -> {
    System.out.println("Running in virtual thread: " + 
        Thread.currentThread());
});

virtualThread.join();
```

### Using Thread.ofVirtual()
```java
Thread virtualThread = Thread.ofVirtual()
    .name("virtual-thread-", 0)
    .start(() -> {
        System.out.println("Virtual thread running");
    });

virtualThread.join();
```

### Using ExecutorService
```java
import java.util.concurrent.Executors;

try (ExecutorService executor = Executors.newVirtualThreadPerTaskExecutor()) {
    for (int i = 0; i < 1000; i++) {
        executor.submit(() -> {
            System.out.println("Task " + Thread.currentThread().getName());
        });
    }
}
```

## Virtual Threads vs Platform Threads
```java
// Platform thread (heavyweight)
Thread platformThread = new Thread(() -> {
    System.out.println("Platform thread");
});
platformThread.start();

// Virtual thread (lightweight)
Thread virtualThread = Thread.startVirtualThread(() -> {
    System.out.println("Virtual thread");
});
virtualThread.join();
```

## Creating Many Virtual Threads
```java
// Can create millions of virtual threads
List<Thread> threads = new ArrayList<>();

for (int i = 0; i < 1_000_000; i++) {
    final int taskId = i;
    Thread thread = Thread.startVirtualThread(() -> {
        System.out.println("Task " + taskId);
    });
    threads.add(thread);
}

// Wait for all
for (Thread thread : threads) {
    thread.join();
}
```

## Virtual Threads with ExecutorService
```java
import java.util.concurrent.*;

try (ExecutorService executor = Executors.newVirtualThreadPerTaskExecutor()) {
    List<Future<String>> futures = new ArrayList<>();
    
    for (int i = 0; i < 10; i++) {
        final int taskId = i;
        Future<String> future = executor.submit(() -> {
            Thread.sleep(1000);
            return "Task " + taskId + " completed";
        });
        futures.add(future);
    }
    
    for (Future<String> future : futures) {
        System.out.println(future.get());
    }
}
```

## Virtual Threads for I/O Operations
```java
try (ExecutorService executor = Executors.newVirtualThreadPerTaskExecutor()) {
    List<String> urls = Arrays.asList(
        "https://api1.example.com",
        "https://api2.example.com",
        "https://api3.example.com"
    );
    
    List<CompletableFuture<String>> futures = urls.stream()
        .map(url -> CompletableFuture.supplyAsync(() -> {
            // I/O operation
            return fetchData(url);
        }, executor))
        .toList();
    
    CompletableFuture.allOf(futures.toArray(new CompletableFuture[0]))
        .join();
}
```

## Thread Factory
```java
ThreadFactory virtualThreadFactory = Thread.ofVirtual()
    .name("worker-", 0)
    .factory();

Thread thread = virtualThreadFactory.newThread(() -> {
    System.out.println("Virtual thread created");
});
thread.start();
```

## Best Practices
1. Use virtual threads for I/O-bound tasks
2. Use platform threads for CPU-intensive tasks
3. Don't pool virtual threads (create as needed)
4. Use Executors.newVirtualThreadPerTaskExecutor()
5. Virtual threads work with existing code
6. Monitor virtual thread usage
7. Use for high-concurrency scenarios
8. Virtual threads are cheap - create many

## Complete Example
```java
import java.util.concurrent.*;

public class VirtualThreadDemo {
    public static void main(String[] args) {
        // Create many virtual threads
        try (ExecutorService executor = 
                Executors.newVirtualThreadPerTaskExecutor()) {
            
            List<Future<Integer>> futures = new ArrayList<>();
            
            for (int i = 0; i < 1000; i++) {
                final int taskId = i;
                Future<Integer> future = executor.submit(() -> {
                    // Simulate I/O operation
                    Thread.sleep(100);
                    return taskId * 2;
                });
                futures.add(future);
            }
            
            // Collect results
            List<Integer> results = futures.stream()
                .map(f -> {
                    try {
                        return f.get();
                    } catch (Exception e) {
                        return -1;
                    }
                })
                .toList();
            
            System.out.println("Completed " + results.size() + " tasks");
        }
    }
}
```


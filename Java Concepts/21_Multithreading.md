# Multithreading

## What is Multithreading?
Multithreading is the ability of a CPU to execute multiple threads concurrently. It allows a program to perform multiple tasks simultaneously.

## Thread vs Process
- **Process**: Independent program with its own memory space
- **Thread**: Lightweight process that shares memory with other threads

## Creating Threads

### Extending Thread Class
```java
class MyThread extends Thread {
    @Override
    public void run() {
        for (int i = 0; i < 5; i++) {
            System.out.println("Thread: " + i);
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}

// Usage
MyThread thread = new MyThread();
thread.start();
```

### Implementing Runnable Interface
```java
class MyRunnable implements Runnable {
    @Override
    public void run() {
        for (int i = 0; i < 5; i++) {
            System.out.println("Runnable: " + i);
        }
    }
}

// Usage
Thread thread = new Thread(new MyRunnable());
thread.start();
```

### Lambda Expression (Java 8+)
```java
Thread thread = new Thread(() -> {
    for (int i = 0; i < 5; i++) {
        System.out.println("Lambda Thread: " + i);
    }
});
thread.start();
```

## Thread States
1. **NEW**: Thread created but not started
2. **RUNNABLE**: Thread is executing
3. **BLOCKED**: Thread waiting for monitor lock
4. **WAITING**: Thread waiting indefinitely
5. **TIMED_WAITING**: Thread waiting for specified time
6. **TERMINATED**: Thread has completed execution

## Thread Methods

### Basic Methods
```java
Thread thread = new Thread(() -> {
    // Thread code
});

thread.start();        // Start thread
thread.join();         // Wait for thread to complete
thread.sleep(1000);    // Sleep for 1 second
thread.interrupt();    // Interrupt thread
thread.isAlive();      // Check if thread is alive
thread.getName();      // Get thread name
thread.setName("MyThread"); // Set thread name
thread.getPriority();  // Get priority
thread.setPriority(Thread.MAX_PRIORITY); // Set priority
```

### Thread Priorities
```java
Thread.MIN_PRIORITY    // 1
Thread.NORM_PRIORITY   // 5 (default)
Thread.MAX_PRIORITY    // 10
```

## Synchronization

### Synchronized Method
```java
class Counter {
    private int count = 0;
    
    public synchronized void increment() {
        count++;
    }
    
    public synchronized int getCount() {
        return count;
    }
}
```

### Synchronized Block
```java
class Counter {
    private int count = 0;
    private Object lock = new Object();
    
    public void increment() {
        synchronized (lock) {
            count++;
        }
    }
}
```

### Thread-Safe Example
```java
class BankAccount {
    private double balance;
    
    public synchronized void deposit(double amount) {
        balance += amount;
    }
    
    public synchronized void withdraw(double amount) {
        if (balance >= amount) {
            balance -= amount;
        }
    }
    
    public synchronized double getBalance() {
        return balance;
    }
}
```

## Wait and Notify

### Producer-Consumer Pattern
```java
class SharedResource {
    private int value;
    private boolean available = false;
    
    public synchronized void produce(int val) {
        while (available) {
            try {
                wait(); // Wait for consumer
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
        value = val;
        available = true;
        notify(); // Notify consumer
    }
    
    public synchronized int consume() {
        while (!available) {
            try {
                wait(); // Wait for producer
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
        available = false;
        notify(); // Notify producer
        return value;
    }
}
```

## Thread Pool

### ExecutorService
```java
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

ExecutorService executor = Executors.newFixedThreadPool(5);

for (int i = 0; i < 10; i++) {
    final int taskId = i;
    executor.submit(() -> {
        System.out.println("Task " + taskId + " executed by " + 
                          Thread.currentThread().getName());
    });
}

executor.shutdown();
```

### Types of Thread Pools
```java
// Fixed thread pool
ExecutorService fixedPool = Executors.newFixedThreadPool(5);

// Cached thread pool
ExecutorService cachedPool = Executors.newCachedThreadPool();

// Single thread executor
ExecutorService singlePool = Executors.newSingleThreadExecutor();

// Scheduled thread pool
ScheduledExecutorService scheduledPool = 
    Executors.newScheduledThreadPool(5);
```

## Callable and Future
```java
import java.util.concurrent.*;

ExecutorService executor = Executors.newFixedThreadPool(5);

Callable<Integer> task = () -> {
    Thread.sleep(2000);
    return 42;
};

Future<Integer> future = executor.submit(task);

try {
    Integer result = future.get(); // Blocks until result is available
    System.out.println("Result: " + result);
} catch (InterruptedException | ExecutionException e) {
    e.printStackTrace();
}

executor.shutdown();
```

## Concurrent Collections

### ConcurrentHashMap
```java
import java.util.concurrent.ConcurrentHashMap;

ConcurrentHashMap<String, Integer> map = new ConcurrentHashMap<>();
map.put("key1", 1);
map.put("key2", 2);
```

### BlockingQueue
```java
import java.util.concurrent.BlockingQueue;
import java.util.concurrent.ArrayBlockingQueue;

BlockingQueue<String> queue = new ArrayBlockingQueue<>(10);

// Producer
queue.put("Item");

// Consumer
String item = queue.take();
```

## Atomic Classes
```java
import java.util.concurrent.atomic.AtomicInteger;

AtomicInteger counter = new AtomicInteger(0);

counter.incrementAndGet();    // Atomic increment
counter.getAndIncrement();    // Get then increment
counter.addAndGet(5);         // Add and get
int value = counter.get();    // Get value
```

## Complete Example
```java
import java.util.concurrent.*;

class Task implements Runnable {
    private int taskId;
    
    public Task(int taskId) {
        this.taskId = taskId;
    }
    
    @Override
    public void run() {
        System.out.println("Task " + taskId + " started by " + 
                          Thread.currentThread().getName());
        try {
            Thread.sleep(2000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        System.out.println("Task " + taskId + " completed");
    }
}

public class MultithreadingDemo {
    public static void main(String[] args) {
        ExecutorService executor = Executors.newFixedThreadPool(3);
        
        for (int i = 1; i <= 5; i++) {
            executor.submit(new Task(i));
        }
        
        executor.shutdown();
        try {
            executor.awaitTermination(1, TimeUnit.MINUTES);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
```

## Best Practices
1. Prefer implementing Runnable over extending Thread
2. Use thread pools instead of creating threads manually
3. Use synchronized blocks over synchronized methods when possible
4. Use concurrent collections for thread-safe operations
5. Avoid deadlocks by acquiring locks in consistent order
6. Use volatile for simple flags
7. Prefer ExecutorService over raw Thread management
8. Always handle InterruptedException properly


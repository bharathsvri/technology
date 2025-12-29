# Advanced Multithreading

## CompletableFuture (Java 8+)
Asynchronous programming with composable futures.

### Basic Usage
```java
import java.util.concurrent.CompletableFuture;
import java.util.concurrent.ExecutionException;

CompletableFuture<String> future = CompletableFuture.supplyAsync(() -> {
    try {
        Thread.sleep(2000);
    } catch (InterruptedException e) {
        e.printStackTrace();
    }
    return "Result";
});

try {
    String result = future.get(); // Blocks until complete
    System.out.println(result);
} catch (InterruptedException | ExecutionException e) {
    e.printStackTrace();
}
```

### Chaining Operations
```java
CompletableFuture<String> future = CompletableFuture
    .supplyAsync(() -> "Hello")
    .thenApply(s -> s + " World")
    .thenApply(String::toUpperCase);

future.thenAccept(System.out::println); // "HELLO WORLD"
```

### Combining Futures
```java
CompletableFuture<String> future1 = CompletableFuture.supplyAsync(() -> "Hello");
CompletableFuture<String> future2 = CompletableFuture.supplyAsync(() -> "World");

CompletableFuture<String> combined = future1
    .thenCombine(future2, (s1, s2) -> s1 + " " + s2);

System.out.println(combined.get()); // "Hello World"
```

### AllOf and AnyOf
```java
CompletableFuture<String> f1 = CompletableFuture.supplyAsync(() -> "Task 1");
CompletableFuture<String> f2 = CompletableFuture.supplyAsync(() -> "Task 2");
CompletableFuture<String> f3 = CompletableFuture.supplyAsync(() -> "Task 3");

// Wait for all
CompletableFuture<Void> all = CompletableFuture.allOf(f1, f2, f3);
all.thenRun(() -> System.out.println("All tasks completed"));

// Wait for any
CompletableFuture<Object> any = CompletableFuture.anyOf(f1, f2, f3);
System.out.println(any.get()); // First completed result
```

### Exception Handling
```java
CompletableFuture<String> future = CompletableFuture
    .supplyAsync(() -> {
        if (Math.random() > 0.5) {
            throw new RuntimeException("Error");
        }
        return "Success";
    })
    .exceptionally(ex -> "Error occurred: " + ex.getMessage())
    .thenApply(String::toUpperCase);

System.out.println(future.get());
```

## Fork/Join Framework
For parallel processing of large tasks.

```java
import java.util.concurrent.ForkJoinPool;
import java.util.concurrent.RecursiveTask;
import java.util.concurrent.RecursiveAction;

class SumTask extends RecursiveTask<Long> {
    private final int[] array;
    private final int start;
    private final int end;
    private static final int THRESHOLD = 1000;
    
    public SumTask(int[] array, int start, int end) {
        this.array = array;
        this.start = start;
        this.end = end;
    }
    
    @Override
    protected Long compute() {
        if (end - start <= THRESHOLD) {
            long sum = 0;
            for (int i = start; i < end; i++) {
                sum += array[i];
            }
            return sum;
        } else {
            int mid = (start + end) / 2;
            SumTask left = new SumTask(array, start, mid);
            SumTask right = new SumTask(array, mid, end);
            left.fork();
            long rightResult = right.compute();
            long leftResult = left.join();
            return leftResult + rightResult;
        }
    }
}

// Usage
ForkJoinPool pool = new ForkJoinPool();
int[] array = new int[10000];
// Initialize array...
SumTask task = new SumTask(array, 0, array.length);
long sum = pool.invoke(task);
```

## CountDownLatch
Waits for one or more threads to complete.

```java
import java.util.concurrent.CountDownLatch;

CountDownLatch latch = new CountDownLatch(3);

for (int i = 0; i < 3; i++) {
    new Thread(() -> {
        // Do work
        latch.countDown(); // Decrement count
    }).start();
}

latch.await(); // Wait until count reaches zero
System.out.println("All threads completed");
```

## CyclicBarrier
Synchronization point where threads wait.

```java
import java.util.concurrent.CyclicBarrier;

CyclicBarrier barrier = new CyclicBarrier(3, () -> {
    System.out.println("All threads reached barrier");
});

for (int i = 0; i < 3; i++) {
    new Thread(() -> {
        try {
            // Do work
            barrier.await(); // Wait for all threads
        } catch (Exception e) {
            e.printStackTrace();
        }
    }).start();
}
```

## Semaphore
Controls access to a resource.

```java
import java.util.concurrent.Semaphore;

Semaphore semaphore = new Semaphore(3); // Allow 3 concurrent accesses

for (int i = 0; i < 10; i++) {
    new Thread(() -> {
        try {
            semaphore.acquire(); // Acquire permit
            // Access resource
            Thread.sleep(1000);
            semaphore.release(); // Release permit
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }).start();
}
```

## Phaser
Flexible synchronization barrier.

```java
import java.util.concurrent.Phaser;

Phaser phaser = new Phaser(3); // 3 parties

for (int i = 0; i < 3; i++) {
    new Thread(() -> {
        phaser.arriveAndAwaitAdvance(); // Wait for all
        // Continue
    }).start();
}
```

## Exchanger
Exchanges data between two threads.

```java
import java.util.concurrent.Exchanger;

Exchanger<String> exchanger = new Exchanger<>();

new Thread(() -> {
    try {
        String data = exchanger.exchange("Data from Thread 1");
        System.out.println("Thread 1 received: " + data);
    } catch (InterruptedException e) {
        e.printStackTrace();
    }
}).start();

new Thread(() -> {
    try {
        String data = exchanger.exchange("Data from Thread 2");
        System.out.println("Thread 2 received: " + data);
    } catch (InterruptedException e) {
        e.printStackTrace();
    }
}).start();
```

## Lock Interface
More flexible than synchronized.

```java
import java.util.concurrent.locks.ReentrantLock;
import java.util.concurrent.locks.ReadWriteLock;
import java.util.concurrent.locks.ReentrantReadWriteLock;

// ReentrantLock
ReentrantLock lock = new ReentrantLock();
try {
    lock.lock();
    // Critical section
} finally {
    lock.unlock();
}

// ReadWriteLock
ReadWriteLock rwLock = new ReentrantReadWriteLock();
rwLock.readLock().lock(); // Multiple readers allowed
try {
    // Read operation
} finally {
    rwLock.readLock().unlock();
}

rwLock.writeLock().lock(); // Exclusive write
try {
    // Write operation
} finally {
    rwLock.writeLock().unlock();
}
```

## StampedLock (Java 8+)
Optimistic locking.

```java
import java.util.concurrent.locks.StampedLock;

StampedLock lock = new StampedLock();

// Optimistic read
long stamp = lock.tryOptimisticRead();
// Read data
if (!lock.validate(stamp)) {
    // Fallback to read lock
    stamp = lock.readLock();
    try {
        // Read data
    } finally {
        lock.unlockRead(stamp);
    }
}
```

## Best Practices
1. Use CompletableFuture for async operations
2. Use Fork/Join for CPU-intensive parallel tasks
3. Choose appropriate synchronization mechanism
4. Avoid deadlocks by consistent lock ordering
5. Use thread-safe collections when possible
6. Monitor thread pool sizes
7. Handle exceptions in async operations
8. Use appropriate timeouts


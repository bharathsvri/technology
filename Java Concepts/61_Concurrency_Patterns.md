# Concurrency Patterns

## Producer-Consumer Pattern
```java
import java.util.concurrent.BlockingQueue;
import java.util.concurrent.LinkedBlockingQueue;

class Producer implements Runnable {
    private BlockingQueue<Integer> queue;
    
    public Producer(BlockingQueue<Integer> queue) {
        this.queue = queue;
    }
    
    @Override
    public void run() {
        try {
            for (int i = 0; i < 10; i++) {
                queue.put(i);
                System.out.println("Produced: " + i);
                Thread.sleep(100);
            }
        } catch (InterruptedException e) {
            Thread.currentThread().interrupt();
        }
    }
}

class Consumer implements Runnable {
    private BlockingQueue<Integer> queue;
    
    public Consumer(BlockingQueue<Integer> queue) {
        this.queue = queue;
    }
    
    @Override
    public void run() {
        try {
            while (true) {
                Integer value = queue.take();
                System.out.println("Consumed: " + value);
                Thread.sleep(200);
            }
        } catch (InterruptedException e) {
            Thread.currentThread().interrupt();
        }
    }
}

// Usage
BlockingQueue<Integer> queue = new LinkedBlockingQueue<>(5);
new Thread(new Producer(queue)).start();
new Thread(new Consumer(queue)).start();
```

## Reader-Writer Lock Pattern
```java
import java.util.concurrent.locks.ReadWriteLock;
import java.util.concurrent.locks.ReentrantReadWriteLock;

class SharedResource {
    private ReadWriteLock lock = new ReentrantReadWriteLock();
    private int data = 0;
    
    public int read() {
        lock.readLock().lock();
        try {
            return data;
        } finally {
            lock.readLock().unlock();
        }
    }
    
    public void write(int value) {
        lock.writeLock().lock();
        try {
            data = value;
        } finally {
            lock.writeLock().unlock();
        }
    }
}
```

## Worker Thread Pattern
```java
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

class WorkerThreadPool {
    private ExecutorService executor;
    
    public WorkerThreadPool(int poolSize) {
        executor = Executors.newFixedThreadPool(poolSize);
    }
    
    public void submitTask(Runnable task) {
        executor.submit(task);
    }
    
    public void shutdown() {
        executor.shutdown();
    }
}

// Usage
WorkerThreadPool pool = new WorkerThreadPool(5);
for (int i = 0; i < 10; i++) {
    final int taskId = i;
    pool.submitTask(() -> {
        System.out.println("Task " + taskId + " executed");
    });
}
pool.shutdown();
```

## Future Pattern
```java
import java.util.concurrent.*;

class FuturePattern {
    public static void main(String[] args) {
        ExecutorService executor = Executors.newSingleThreadExecutor();
        
        Future<String> future = executor.submit(() -> {
            Thread.sleep(2000);
            return "Task completed";
        });
        
        try {
            String result = future.get(5, TimeUnit.SECONDS);
            System.out.println(result);
        } catch (TimeoutException e) {
            future.cancel(true);
            System.out.println("Task timed out");
        } catch (Exception e) {
            e.printStackTrace();
        }
        
        executor.shutdown();
    }
}
```

## Thread Pool Pattern
```java
import java.util.concurrent.*;

class ThreadPoolManager {
    private ExecutorService executor;
    
    public ThreadPoolManager() {
        executor = Executors.newCachedThreadPool();
    }
    
    public <T> Future<T> submit(Callable<T> task) {
        return executor.submit(task);
    }
    
    public void execute(Runnable task) {
        executor.execute(task);
    }
    
    public void shutdown() {
        executor.shutdown();
        try {
            if (!executor.awaitTermination(60, TimeUnit.SECONDS)) {
                executor.shutdownNow();
            }
        } catch (InterruptedException e) {
            executor.shutdownNow();
        }
    }
}
```

## Double-Checked Locking Pattern
```java
class Singleton {
    private static volatile Singleton instance;
    
    private Singleton() {}
    
    public static Singleton getInstance() {
        if (instance == null) {
            synchronized (Singleton.class) {
                if (instance == null) {
                    instance = new Singleton();
                }
            }
        }
        return instance;
    }
}
```

## Balking Pattern
```java
class BalkingPattern {
    private boolean initialized = false;
    
    public synchronized void initialize() {
        if (initialized) {
            return; // Balking
        }
        // Initialize
        initialized = true;
    }
    
    public void doWork() {
        if (!initialized) {
            initialize();
        }
        // Do work
    }
}
```

## Guarded Suspension Pattern
```java
import java.util.concurrent.locks.Condition;
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

class GuardedSuspension {
    private Lock lock = new ReentrantLock();
    private Condition condition = lock.newCondition();
    private boolean ready = false;
    
    public void waitForReady() throws InterruptedException {
        lock.lock();
        try {
            while (!ready) {
                condition.await();
            }
        } finally {
            lock.unlock();
        }
    }
    
    public void setReady() {
        lock.lock();
        try {
            ready = true;
            condition.signalAll();
        } finally {
            lock.unlock();
        }
    }
}
```

## Best Practices
1. Use thread pools instead of creating threads manually
2. Prefer concurrent collections
3. Use appropriate synchronization mechanisms
4. Avoid deadlocks with consistent lock ordering
5. Handle interruptions properly
6. Use volatile for simple flags
7. Consider using CompletableFuture for async operations
8. Test concurrent code thoroughly


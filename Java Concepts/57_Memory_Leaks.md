# Memory Leaks

## What is a Memory Leak?
A memory leak occurs when objects are no longer needed but still referenced, preventing garbage collection.

## Common Causes

### 1. Static Collections
```java
class LeakyClass {
    private static List<Object> cache = new ArrayList<>();
    
    public void addToCache(Object obj) {
        cache.add(obj); // Never removed
    }
}

// Solution: Clear or limit size
class FixedClass {
    private static final int MAX_SIZE = 100;
    private static List<Object> cache = new ArrayList<>();
    
    public void addToCache(Object obj) {
        if (cache.size() >= MAX_SIZE) {
            cache.remove(0); // Remove oldest
        }
        cache.add(obj);
    }
}
```

### 2. Unclosed Resources
```java
// Memory leak
public void readFile() {
    FileInputStream fis = new FileInputStream("file.txt");
    // Forgot to close
}

// Solution: Use try-with-resources
public void readFile() {
    try (FileInputStream fis = new FileInputStream("file.txt")) {
        // Use file
    } // Automatically closed
}
```

### 3. Listeners Not Removed
```java
class EventSource {
    private List<EventListener> listeners = new ArrayList<>();
    
    public void addListener(EventListener listener) {
        listeners.add(listener);
    }
    
    // Missing: removeListener method
}

// Solution: Always provide removal
public void removeListener(EventListener listener) {
    listeners.remove(listener);
}
```

### 4. ThreadLocal Variables
```java
class ThreadLocalLeak {
    private static ThreadLocal<Object> threadLocal = new ThreadLocal<>();
    
    public void setValue(Object value) {
        threadLocal.set(value);
        // Never removed
    }
}

// Solution: Always remove
public void cleanup() {
    threadLocal.remove();
}
```

### 5. Inner Classes Holding Outer Reference
```java
class Outer {
    private byte[] largeData = new byte[1000000];
    
    class Inner {
        // Holds reference to Outer
    }
    
    Inner getInner() {
        return new Inner();
    }
}

// Solution: Use static inner class if outer reference not needed
static class Inner {
    // No reference to Outer
}
```

### 6. Circular References (with Finalizers)
```java
class Node {
    Node next;
    
    @Override
    protected void finalize() {
        // Finalizer can prevent GC
    }
}

// Solution: Avoid finalizers, use try-finally or try-with-resources
```

## Detecting Memory Leaks

### Using JVisualVM
```bash
jvisualvm
# Monitor heap usage
# Take heap dump
# Analyze objects
```

### Using jmap
```bash
# Generate heap dump
jmap -dump:format=b,file=heap.bin <pid>

# Analyze with jhat or Eclipse MAT
jhat heap.bin
```

### Programmatic Monitoring
```java
import java.lang.management.*;

MemoryMXBean memoryBean = ManagementFactory.getMemoryMXBean();
MemoryUsage heapUsage = memoryBean.getHeapMemoryUsage();

long used = heapUsage.getUsed();
long max = heapUsage.getMax();
double usagePercent = (double) used / max * 100;

if (usagePercent > 80) {
    System.out.println("High memory usage: " + usagePercent + "%");
}
```

## Prevention Strategies

### 1. Use Weak References
```java
import java.lang.ref.WeakReference;

WeakReference<Object> weakRef = new WeakReference<>(object);
// Object can be GC'd when only weakly referenced
```

### 2. Use Soft References for Caching
```java
import java.lang.ref.SoftReference;

SoftReference<Object> softRef = new SoftReference<>(object);
// GC'd only when memory is low
```

### 3. Implement Proper Cleanup
```java
class Resource implements AutoCloseable {
    @Override
    public void close() {
        // Cleanup resources
    }
}

// Use try-with-resources
try (Resource resource = new Resource()) {
    // Use resource
} // Automatically closed
```

### 4. Limit Collection Sizes
```java
class BoundedCache {
    private static final int MAX_SIZE = 1000;
    private Map<String, Object> cache = new LinkedHashMap<String, Object>() {
        @Override
        protected boolean removeEldestEntry(Map.Entry eldest) {
            return size() > MAX_SIZE;
        }
    };
}
```

## Best Practices
1. Always close resources (use try-with-resources)
2. Remove listeners when done
3. Clear collections when no longer needed
4. Use WeakReference for caches
5. Avoid static collections that grow indefinitely
6. Remove ThreadLocal values
7. Use bounded collections
8. Profile regularly to detect leaks
9. Monitor memory usage
10. Use appropriate reference types

## Memory Leak Checklist
- [ ] All resources properly closed
- [ ] Listeners removed when done
- [ ] Collections cleared or bounded
- [ ] ThreadLocal values removed
- [ ] Static collections limited
- [ ] No unnecessary object references
- [ ] Weak/Soft references used appropriately
- [ ] Regular memory profiling
- [ ] Heap dumps analyzed
- [ ] Memory usage monitored

## Complete Example
```java
import java.util.*;
import java.lang.ref.*;

class MemoryLeakDemo {
    // Bad: Static collection that grows
    private static List<Object> badCache = new ArrayList<>();
    
    // Good: Bounded cache
    private static final int MAX_CACHE_SIZE = 100;
    private static Map<String, SoftReference<Object>> goodCache = 
        new LinkedHashMap<String, SoftReference<Object>>() {
            @Override
            protected boolean removeEldestEntry(Map.Entry eldest) {
                return size() > MAX_CACHE_SIZE;
            }
        };
    
    public static void main(String[] args) {
        // Monitor memory
        MemoryMXBean memoryBean = ManagementFactory.getMemoryMXBean();
        
        // Simulate memory leak
        for (int i = 0; i < 10000; i++) {
            badCache.add(new byte[10000]); // Memory leak
        }
        
        // Check memory
        MemoryUsage heapUsage = memoryBean.getHeapMemoryUsage();
        System.out.println("Memory used: " + 
            heapUsage.getUsed() / 1024 / 1024 + " MB");
        
        // Cleanup
        badCache.clear();
        System.gc();
        
        System.out.println("Memory after cleanup: " + 
            memoryBean.getHeapMemoryUsage().getUsed() / 1024 / 1024 + " MB");
    }
}
```


# Garbage Collection

## What is Garbage Collection?
Garbage Collection (GC) automatically manages memory by reclaiming objects that are no longer referenced.

## Heap Memory Structure

### Generations
```
Heap
├── Young Generation
│   ├── Eden Space
│   └── Survivor Spaces (S0, S1)
└── Old Generation (Tenured)
```

## GC Process

### Minor GC
- Occurs in Young Generation
- Fast, frequent
- Surviving objects moved to Survivor space or Old Generation

### Major GC (Full GC)
- Occurs in Old Generation
- Slower, less frequent
- More expensive operation

## Types of Garbage Collectors

### Serial GC
Single-threaded, suitable for small applications.

```bash
java -XX:+UseSerialGC MyApp
```

### Parallel GC
Multi-threaded, default for server applications.

```bash
java -XX:+UseParallelGC MyApp
java -XX:+UseParallelOldGC MyApp
```

### G1 GC (Garbage First)
Low-latency collector for large heaps.

```bash
java -XX:+UseG1GC MyApp
java -XX:MaxGCPauseMillis=200 MyApp
```

### ZGC (Java 11+)
Ultra-low latency collector.

```bash
java -XX:+UseZGC MyApp
```

### Shenandoah GC (Java 12+)
Low-pause-time collector.

```bash
java -XX:+UseShenandoahGC MyApp
```

## GC Tuning Options

### Heap Size
```bash
-Xms512m          # Initial heap size
-Xmx2048m         # Maximum heap size
-XX:NewRatio=2     # Ratio of old to young generation
-XX:SurvivorRatio=8 # Ratio of Eden to Survivor space
```

### GC Logging
```bash
-XX:+PrintGCDetails
-XX:+PrintGCDateStamps
-Xloggc:gc.log
-XX:+UseGCLogFileRotation
-XX:NumberOfGCLogFiles=5
-XX:GCLogFileSize=20M
```

### Modern GC Logging (Java 9+)
```bash
-Xlog:gc*:file=gc.log:time,level,tags
```

## Monitoring GC

### Programmatic Monitoring
```java
import java.lang.management.*;

MemoryMXBean memoryBean = ManagementFactory.getMemoryMXBean();
MemoryUsage heapUsage = memoryBean.getHeapMemoryUsage();

System.out.println("Used: " + heapUsage.getUsed() / 1024 / 1024 + " MB");
System.out.println("Max: " + heapUsage.getMax() / 1024 / 1024 + " MB");
System.out.println("Committed: " + heapUsage.getCommitted() / 1024 / 1024 + " MB");
```

### GC Statistics
```java
import java.lang.management.*;

List<GarbageCollectorMXBean> gcBeans = 
    ManagementFactory.getGarbageCollectorMXBeans();

for (GarbageCollectorMXBean gcBean : gcBeans) {
    System.out.println("GC Name: " + gcBean.getName());
    System.out.println("Collections: " + gcBean.getCollectionCount());
    System.out.println("Time: " + gcBean.getCollectionTime() + " ms");
}
```

## Forcing GC (Not Recommended)
```java
// Request GC (not guaranteed)
System.gc();

// Or
Runtime.getRuntime().gc();
```

## Memory Leaks Prevention

### Common Causes
1. **Static Collections**: Growing collections that never clear
2. **Listeners**: Not removing event listeners
3. **Thread Local Variables**: Not cleaning up
4. **Circular References**: With finalizers (rare)

### Best Practices
```java
// Clear collections when done
List<Object> list = new ArrayList<>();
// Use list...
list.clear();

// Remove listeners
button.removeActionListener(listener);

// Close resources
try (Resource resource = new Resource()) {
    // Use resource
} // Automatically closed
```

## Complete Example
```java
import java.lang.management.*;

public class GCDemo {
    public static void main(String[] args) {
        // Get memory info
        MemoryMXBean memoryBean = ManagementFactory.getMemoryMXBean();
        MemoryUsage heapUsage = memoryBean.getHeapMemoryUsage();
        
        System.out.println("=== Heap Memory ===");
        System.out.println("Used: " + 
            heapUsage.getUsed() / 1024 / 1024 + " MB");
        System.out.println("Max: " + 
            heapUsage.getMax() / 1024 / 1024 + " MB");
        
        // Get GC info
        List<GarbageCollectorMXBean> gcBeans = 
            ManagementFactory.getGarbageCollectorMXBeans();
        
        System.out.println("\n=== Garbage Collectors ===");
        for (GarbageCollectorMXBean gcBean : gcBeans) {
            System.out.println("Name: " + gcBean.getName());
            System.out.println("Collections: " + gcBean.getCollectionCount());
            System.out.println("Time: " + gcBean.getCollectionTime() + " ms");
        }
    }
}
```

## Best Practices
1. Don't call System.gc() explicitly
2. Tune heap size based on application needs
3. Choose appropriate GC algorithm
4. Monitor GC logs regularly
5. Avoid memory leaks
6. Use try-with-resources
7. Clear large collections when done
8. Profile application to understand memory usage


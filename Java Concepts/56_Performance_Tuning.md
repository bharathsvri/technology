# Performance Tuning

## Performance Optimization Strategies

### 1. String Optimization
```java
// Bad: Creates many temporary objects
String result = "";
for (int i = 0; i < 1000; i++) {
    result += "text";
}

// Good: Use StringBuilder
StringBuilder sb = new StringBuilder();
for (int i = 0; i < 1000; i++) {
    sb.append("text");
}
String result = sb.toString();
```

### 2. Collection Optimization
```java
// Specify initial capacity for known size
List<String> list = new ArrayList<>(1000);

// Use appropriate collection type
Set<String> set = new HashSet<>(); // O(1) lookup
// vs
List<String> list = new ArrayList<>(); // O(n) lookup

// Pre-size collections when possible
Map<String, Integer> map = new HashMap<>(expectedSize);
```

### 3. Loop Optimization
```java
// Cache array length
int[] array = new int[1000];
int len = array.length; // Cache
for (int i = 0; i < len; i++) {
    // Process
}

// Use enhanced for loop when index not needed
for (int value : array) {
    // Process
}
```

### 4. Avoid Unnecessary Object Creation
```java
// Bad: Creates new object each time
for (int i = 0; i < 1000; i++) {
    Date date = new Date();
    // Use date
}

// Good: Reuse object
Date date = new Date();
for (int i = 0; i < 1000; i++) {
    // Use date
}
```

### 5. Use Primitives When Possible
```java
// Bad: Autoboxing overhead
List<Integer> numbers = new ArrayList<>();
for (int i = 0; i < 1000; i++) {
    numbers.add(i); // Autoboxing
}

// Good: Use primitive arrays
int[] numbers = new int[1000];
for (int i = 0; i < 1000; i++) {
    numbers[i] = i;
}
```

## JVM Tuning

### Heap Size
```bash
# Set initial and max heap
java -Xms512m -Xmx2048m MyApp

# For large applications
java -Xms2g -Xmx4g MyApp
```

### Garbage Collection
```bash
# Use G1 GC for low latency
java -XX:+UseG1GC -XX:MaxGCPauseMillis=200 MyApp

# Use Parallel GC for throughput
java -XX:+UseParallelGC MyApp
```

### JIT Compiler
```bash
# Enable JIT optimizations
java -XX:+TieredCompilation MyApp

# Disable JIT (for debugging)
java -Xint MyApp
```

## Profiling Tools

### JVisualVM
```bash
# Included with JDK
jvisualvm
```

### JProfiler
Commercial profiler for detailed analysis.

### Java Flight Recorder (JFR)
```bash
# Enable JFR
java -XX:+FlightRecorder MyApp

# Record
jcmd <pid> JFR.start name=myrecording
jcmd <pid> JFR.stop name=myrecording
```

## Memory Optimization

### Object Pooling
```java
class ObjectPool<T> {
    private Queue<T> pool = new LinkedList<>();
    
    public T acquire() {
        return pool.poll();
    }
    
    public void release(T obj) {
        pool.offer(obj);
    }
}
```

### Weak References
```java
import java.lang.ref.WeakReference;

WeakReference<Object> weakRef = new WeakReference<>(object);
Object obj = weakRef.get(); // May return null if GC'd
```

## Algorithm Optimization

### Choose Right Data Structure
```java
// O(1) lookup
Map<String, Integer> map = new HashMap<>();

// O(log n) lookup
TreeMap<String, Integer> treeMap = new TreeMap<>();

// O(n) lookup
List<String> list = new ArrayList<>();
```

### Avoid Premature Optimization
```java
// Write clear, correct code first
// Then profile and optimize hotspots
```

## Best Practices
1. Profile before optimizing
2. Focus on bottlenecks (80/20 rule)
3. Measure performance improvements
4. Use appropriate data structures
5. Minimize object creation
6. Use primitives when possible
7. Cache frequently accessed data
8. Consider concurrency for CPU-intensive tasks
9. Tune JVM parameters based on workload
10. Monitor and measure continuously

## Performance Checklist
- [ ] Use StringBuilder for string concatenation
- [ ] Pre-size collections
- [ ] Choose appropriate data structures
- [ ] Avoid unnecessary object creation
- [ ] Use primitives instead of wrappers
- [ ] Cache expensive computations
- [ ] Optimize loops
- [ ] Tune JVM parameters
- [ ] Profile and measure
- [ ] Use parallel processing when appropriate


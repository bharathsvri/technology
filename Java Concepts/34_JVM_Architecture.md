# JVM Architecture

## What is JVM?
JVM (Java Virtual Machine) is an abstract machine that provides runtime environment for Java bytecode execution. It makes Java platform-independent.

## JVM Architecture Components

### 1. Class Loader Subsystem
Loads class files into memory.

**Types of Class Loaders:**
- **Bootstrap Class Loader**: Loads core Java classes (rt.jar)
- **Extension Class Loader**: Loads extension classes
- **Application Class Loader**: Loads application classes

```java
// Get class loader
ClassLoader loader = MyClass.class.getClassLoader();
System.out.println(loader);
```

### 2. Runtime Data Areas

#### Method Area
Stores class structure, methods, and static variables.

#### Heap Area
Stores objects and arrays.

**Heap Structure:**
- **Young Generation**
  - Eden Space
  - Survivor Space (S0, S1)
- **Old Generation** (Tenured Space)
- **Permanent Generation** (Java 7 and earlier)
- **Metaspace** (Java 8+)

```java
// Heap memory info
Runtime runtime = Runtime.getRuntime();
long maxMemory = runtime.maxMemory();
long totalMemory = runtime.totalMemory();
long freeMemory = runtime.freeMemory();
```

#### Stack Area
Stores method calls and local variables.

**Stack Frame Contains:**
- Local variables
- Operand stack
- Frame data

#### PC Register
Stores address of currently executing instruction.

#### Native Method Stack
Stores native method information.

### 3. Execution Engine

#### Interpreter
Reads and executes bytecode line by line.

#### JIT Compiler
Compiles frequently used bytecode to native code for better performance.

#### Garbage Collector
Automatically manages memory by removing unused objects.

## Garbage Collection

### Types of GC

#### Serial GC
Single-threaded, suitable for small applications.

```bash
java -XX:+UseSerialGC MyApp
```

#### Parallel GC
Multi-threaded, default for server applications.

```bash
java -XX:+UseParallelGC MyApp
```

#### G1 GC
Low-latency garbage collector for large heaps.

```bash
java -XX:+UseG1GC MyApp
```

### GC Process
1. **Mark**: Identify live objects
2. **Sweep**: Remove dead objects
3. **Compact**: Defragment memory

## Memory Management

### Heap Memory
```java
// Check memory
Runtime runtime = Runtime.getRuntime();
long maxMemory = runtime.maxMemory() / 1024 / 1024; // MB
long totalMemory = runtime.totalMemory() / 1024 / 1024;
long freeMemory = runtime.freeMemory() / 1024 / 1024;
long usedMemory = totalMemory - freeMemory;

System.out.println("Max Memory: " + maxMemory + " MB");
System.out.println("Total Memory: " + totalMemory + " MB");
System.out.println("Free Memory: " + freeMemory + " MB");
System.out.println("Used Memory: " + usedMemory + " MB");
```

### Setting Heap Size
```bash
# Set initial and maximum heap size
java -Xms512m -Xmx1024m MyApp

# -Xms: Initial heap size
# -Xmx: Maximum heap size
```

## JVM Options

### Memory Options
```bash
-Xms<size>        # Initial heap size
-Xmx<size>        # Maximum heap size
-Xss<size>        # Thread stack size
-XX:MetaspaceSize=<size>  # Metaspace size
```

### GC Options
```bash
-XX:+UseG1GC              # Use G1 garbage collector
-XX:MaxGCPauseMillis=200  # Max GC pause time
-XX:+PrintGCDetails       # Print GC details
```

### Debug Options
```bash
-XX:+HeapDumpOnOutOfMemoryError  # Create heap dump on OOM
-XX:HeapDumpPath=/path/to/dump   # Heap dump path
-verbose:gc                      # Verbose GC output
```

## JVM Tuning Tips
1. Set appropriate heap size (-Xms, -Xmx)
2. Choose right garbage collector
3. Monitor GC logs
4. Tune GC parameters based on application
5. Use profiling tools (JVisualVM, JProfiler)
6. Monitor memory usage
7. Optimize object creation
8. Avoid memory leaks

## Complete Example
```java
public class JVMDemo {
    public static void main(String[] args) {
        Runtime runtime = Runtime.getRuntime();
        
        System.out.println("=== JVM Memory Information ===");
        System.out.println("Max Memory: " + 
            runtime.maxMemory() / 1024 / 1024 + " MB");
        System.out.println("Total Memory: " + 
            runtime.totalMemory() / 1024 / 1024 + " MB");
        System.out.println("Free Memory: " + 
            runtime.freeMemory() / 1024 / 1024 + " MB");
        System.out.println("Used Memory: " + 
            (runtime.totalMemory() - runtime.freeMemory()) / 1024 / 1024 + " MB");
        
        System.out.println("\n=== Processor Information ===");
        System.out.println("Available Processors: " + 
            runtime.availableProcessors());
        
        // Force garbage collection (not recommended in production)
        System.gc();
    }
}
```

## Best Practices
1. Monitor JVM performance regularly
2. Set appropriate heap sizes
3. Choose GC algorithm based on application needs
4. Use profiling tools for optimization
5. Monitor GC logs for issues
6. Tune JVM parameters based on workload
7. Keep JVM updated
8. Understand your application's memory patterns


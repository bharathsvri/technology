# JVM Architecture Deep Dive

## JVM in One Line
JVM executes Java bytecode, manages memory, and provides platform-independent runtime behavior.

## End-to-End Flow
1. Write source code (`.java`)
2. Compile with `javac` to bytecode (`.class`)
3. JVM loads classes
4. Bytecode verifier validates safety
5. Interpreter starts execution
6. JIT compiles hot methods to native code
7. Garbage Collector reclaims unused memory

## JVM Major Components

### 1. Class Loader Subsystem
- **Bootstrap ClassLoader**: Loads core Java classes
- **Platform ClassLoader**: Loads platform classes
- **Application ClassLoader**: Loads app classpath classes

### 2. Runtime Data Areas

#### Method Area (Metaspace)
- Stores class metadata, runtime constant pool, method data, static fields.

#### Heap
- Stores objects and arrays.
- Shared by all threads.
- Generational model:
  - Young Generation (Eden + Survivor spaces)
  - Old Generation

#### Java Stack
- One stack per thread.
- Contains stack frames for each method call:
  - Local variables
  - Operand stack
  - Frame metadata

#### PC Register
- Tracks current instruction address for each thread.

#### Native Method Stack
- Supports JNI/native method execution.

## Execution Engine

### Interpreter
- Executes bytecode instruction-by-instruction.
- Fast startup, slower long-running performance.

### JIT Compiler
- Compiles frequently used ("hot") methods to native machine code.
- Improves throughput after warm-up.

### Garbage Collector
- Automatically frees memory from unreachable objects.
- Prevents manual memory free/delete errors.

## Class Loading Lifecycle
1. **Loading**
2. **Linking**
   - Verification
   - Preparation
   - Resolution
3. **Initialization**

## Important JVM Flags
```bash
# Heap sizing
-Xms512m -Xmx2g

# Stack size per thread
-Xss1m

# GC options
-XX:+UseG1GC
-XX:+UseZGC
-XX:+UseShenandoahGC

# Diagnostics
-Xlog:gc*
-XX:+HeapDumpOnOutOfMemoryError
```

## Common Garbage Collectors
- **G1 GC**: Balanced default for many workloads
- **ZGC**: Very low pause times, large heaps
- **Shenandoah**: Low pause collector
- **Parallel GC**: High throughput focus

## Performance Concepts
- Throughput vs latency trade-off
- Allocation rate affects GC frequency
- Object lifetime pattern affects generational efficiency
- Excessive object creation can increase GC pressure

## Useful Monitoring Tools
- JDK Mission Control (JMC)
- Java Flight Recorder (JFR)
- `jcmd`, `jstat`, `jmap`, `jstack`

## Minimal Memory Inspection Example
```java
Runtime rt = Runtime.getRuntime();
long max = rt.maxMemory() / (1024 * 1024);
long total = rt.totalMemory() / (1024 * 1024);
long free = rt.freeMemory() / (1024 * 1024);
long used = total - free;

System.out.println("Max MB  : " + max);
System.out.println("Total MB: " + total);
System.out.println("Used MB : " + used);
System.out.println("Free MB : " + free);
```

## Interview Checklist
- Difference between JDK, JRE, JVM
- Heap vs Stack
- Class loading phases
- Interpreter vs JIT
- Strong and weak references
- GC basics and collector choices

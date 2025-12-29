# Process API

## What is Process API?
The Process API allows Java applications to create and manage operating system processes.

## Creating Processes

### ProcessBuilder
```java
import java.io.*;

// Simple command
ProcessBuilder pb = new ProcessBuilder("echo", "Hello World");
Process process = pb.start();

// Read output
try (BufferedReader reader = new BufferedReader(
        new InputStreamReader(process.getInputStream()))) {
    String line;
    while ((line = reader.readLine()) != null) {
        System.out.println(line);
    }
}
```

### Redirecting Output
```java
import java.io.*;

ProcessBuilder pb = new ProcessBuilder("ls", "-la");
pb.redirectOutput(new File("output.txt"));
pb.redirectError(new File("error.txt"));
Process process = pb.start();
process.waitFor();
```

### Setting Environment Variables
```java
ProcessBuilder pb = new ProcessBuilder("command");
Map<String, String> env = pb.environment();
env.put("VAR1", "value1");
env.put("VAR2", "value2");
Process process = pb.start();
```

### Setting Working Directory
```java
ProcessBuilder pb = new ProcessBuilder("command");
pb.directory(new File("/path/to/directory"));
Process process = pb.start();
```

## Process Information (Java 9+)

### Getting Process Info
```java
import java.lang.ProcessHandle;

ProcessHandle current = ProcessHandle.current();

// Process info
ProcessHandle.Info info = current.info();
System.out.println("Command: " + info.command().orElse("N/A"));
System.out.println("Arguments: " + info.arguments().orElse(new String[]{}));
System.out.println("Start time: " + info.startInstant().orElse(null));
System.out.println("CPU time: " + info.totalCpuDuration().orElse(null));
System.out.println("User: " + info.user().orElse("N/A"));
```

### Listing All Processes
```java
import java.lang.ProcessHandle;

ProcessHandle.allProcesses()
    .forEach(process -> {
        ProcessHandle.Info info = process.info();
        System.out.println("PID: " + process.pid());
        System.out.println("Command: " + 
            info.command().orElse("N/A"));
    });
```

### Process Tree
```java
import java.lang.ProcessHandle;

ProcessHandle current = ProcessHandle.current();

// Get parent
ProcessHandle parent = current.parent().orElse(null);
if (parent != null) {
    System.out.println("Parent PID: " + parent.pid());
}

// Get children
current.children().forEach(child -> {
    System.out.println("Child PID: " + child.pid());
});

// Get descendants
current.descendants().forEach(descendant -> {
    System.out.println("Descendant PID: " + descendant.pid());
});
```

## Waiting for Process

### waitFor()
```java
ProcessBuilder pb = new ProcessBuilder("command");
Process process = pb.start();

int exitCode = process.waitFor();
System.out.println("Exit code: " + exitCode);
```

### waitFor with Timeout (Java 9+)
```java
import java.util.concurrent.TimeUnit;

ProcessBuilder pb = new ProcessBuilder("command");
Process process = pb.start();

boolean finished = process.waitFor(5, TimeUnit.SECONDS);
if (finished) {
    System.out.println("Process completed");
} else {
    process.destroy();
    System.out.println("Process timed out");
}
```

## Destroying Processes

### destroy()
```java
Process process = pb.start();
process.destroy(); // Terminate process
```

### destroyForcibly() (Java 8+)
```java
Process process = pb.start();
process.destroy(); // Try graceful termination
if (!process.waitFor(5, TimeUnit.SECONDS)) {
    process.destroyForcibly(); // Force kill
}
```

## Process Completion (Java 9+)
```java
import java.util.concurrent.CompletableFuture;

ProcessBuilder pb = new ProcessBuilder("command");
Process process = pb.start();

ProcessHandle handle = process.toHandle();

CompletableFuture<ProcessHandle> future = handle.onExit();
future.thenRun(() -> {
    System.out.println("Process " + handle.pid() + " has terminated");
});
```

## Complete Example
```java
import java.io.*;
import java.util.concurrent.TimeUnit;

public class ProcessDemo {
    public static void main(String[] args) {
        try {
            // Create process
            ProcessBuilder pb = new ProcessBuilder("java", "-version");
            pb.redirectErrorStream(true);
            
            Process process = pb.start();
            
            // Read output
            try (BufferedReader reader = new BufferedReader(
                    new InputStreamReader(process.getInputStream()))) {
                String line;
                while ((line = reader.readLine()) != null) {
                    System.out.println(line);
                }
            }
            
            // Wait for completion
            boolean finished = process.waitFor(10, TimeUnit.SECONDS);
            if (finished) {
                int exitCode = process.exitValue();
                System.out.println("Exit code: " + exitCode);
            } else {
                process.destroyForcibly();
                System.out.println("Process timed out");
            }
            
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

## Best Practices
1. Always handle process output streams
2. Use waitFor() with timeout
3. Clean up processes properly
4. Handle exceptions
5. Redirect streams appropriately
6. Use ProcessHandle for process management (Java 9+)
7. Be careful with process destruction
8. Consider security implications


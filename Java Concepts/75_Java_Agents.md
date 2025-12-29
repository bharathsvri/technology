# Java Agents

## What are Java Agents?
Java agents allow modification of bytecode at runtime using the Instrumentation API.

## Basic Agent
```java
import java.lang.instrument.Instrumentation;

public class MyAgent {
    public static void premain(String agentArgs, Instrumentation inst) {
        System.out.println("Agent started with args: " + agentArgs);
        
        // Add transformer
        inst.addTransformer(new MyTransformer());
    }
}

class MyTransformer implements ClassFileTransformer {
    @Override
    public byte[] transform(ClassLoader loader, String className,
                          Class<?> classBeingRedefined,
                          ProtectionDomain protectionDomain,
                          byte[] classfileBuffer) {
        // Transform bytecode
        return classfileBuffer;
    }
}
```

## Manifest File
```manifest
Manifest-Version: 1.0
Premain-Class: MyAgent
```

## Running with Agent
```bash
java -javaagent:myagent.jar MyApplication
```

## Agentmain (Dynamic Loading)
```java
public class MyAgent {
    public static void agentmain(String agentArgs, Instrumentation inst) {
        // Attach to running JVM
    }
}
```

## Use Cases
- Performance monitoring
- Code coverage
- Profiling
- Logging
- Security
- Bytecode manipulation

## Best Practices
1. Keep agents lightweight
2. Handle exceptions properly
3. Don't modify core classes
4. Test thoroughly
5. Document agent behavior
6. Use appropriate tools (ASM, Javassist)
7. Consider performance impact
8. Follow security guidelines


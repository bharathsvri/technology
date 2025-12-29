# Bytecode

## What is Bytecode?
Bytecode is the intermediate representation of Java code that the JVM executes.

## Viewing Bytecode

### Using javap
```bash
# Disassemble class file
javap -c MyClass.class

# Verbose output
javap -v MyClass.class

# Show all members
javap -p MyClass.class

# Show public members only
javap MyClass.class
```

### Example Bytecode
```java
// Java code
public int add(int a, int b) {
    return a + b;
}

// Bytecode (simplified)
public int add(int, int);
  Code:
    0: iload_1    // Load first parameter
    1: iload_2    // Load second parameter
    2: iadd       // Add integers
    3: ireturn    // Return integer
```

## Common Bytecode Instructions

### Load/Store
- `iload`, `lload`, `fload`, `dload`, `aload` - Load from local variable
- `istore`, `lstore`, `fstore`, `dstore`, `astore` - Store to local variable
- `iconst_0`, `iconst_1`, `lconst_0`, etc. - Load constant

### Arithmetic
- `iadd`, `isub`, `imul`, `idiv`, `irem` - Integer arithmetic
- `ladd`, `lsub`, `lmul`, `ldiv`, `lrem` - Long arithmetic
- `fadd`, `fsub`, `fmul`, `fdiv`, `frem` - Float arithmetic
- `dadd`, `dsub`, `dmul`, `ddiv`, `drem` - Double arithmetic

### Control Flow
- `ifeq`, `ifne`, `iflt`, `ifge`, `ifgt`, `ifle` - Conditional branches
- `if_icmpeq`, `if_icmpne`, etc. - Integer comparison branches
- `goto` - Unconditional branch
- `tableswitch`, `lookupswitch` - Switch statements

### Method Calls
- `invokevirtual` - Virtual method call
- `invokestatic` - Static method call
- `invokeinterface` - Interface method call
- `invokespecial` - Constructor or private method call

### Object Operations
- `new` - Create new object
- `getfield`, `putfield` - Access instance fields
- `getstatic`, `putstatic` - Access static fields
- `checkcast` - Type check
- `instanceof` - Type check

## Understanding Bytecode

### Simple Method
```java
// Java
public void method() {
    int x = 10;
    int y = 20;
    int z = x + y;
}

// Bytecode
0: bipush 10      // Push 10 onto stack
2: istore_1       // Store in local variable 1 (x)
3: bipush 20      // Push 20 onto stack
5: istore_2       // Store in local variable 2 (y)
6: iload_1        // Load x
7: iload_2        // Load y
8: iadd           // Add
9: istore_3       // Store result in z
10: return
```

### Conditional
```java
// Java
public int max(int a, int b) {
    if (a > b) {
        return a;
    }
    return b;
}

// Bytecode
0: iload_1        // Load a
1: iload_2        // Load b
2: if_icmple 7     // If a <= b, goto 7
5: iload_1        // Load a
6: ireturn        // Return a
7: iload_2        // Load b
8: ireturn        // Return b
```

## Bytecode Manipulation

### ASM Library
```xml
<dependency>
    <groupId>org.ow2.asm</groupId>
    <artifactId>asm</artifactId>
    <version>9.6</version>
</dependency>
```

### Basic ASM Usage
```java
import org.objectweb.asm.*;

class MyClassVisitor extends ClassVisitor {
    public MyClassVisitor(ClassVisitor cv) {
        super(Opcodes.ASM9, cv);
    }
    
    @Override
    public MethodVisitor visitMethod(int access, String name, 
            String descriptor, String signature, String[] exceptions) {
        System.out.println("Method: " + name);
        return super.visitMethod(access, name, descriptor, signature, exceptions);
    }
}
```

## Best Practices
1. Understand bytecode for performance tuning
2. Use javap to analyze compiled code
3. Learn common bytecode patterns
4. Use bytecode manipulation tools carefully
5. Understand JIT compilation impact
6. Profile before optimizing bytecode
7. Keep bytecode readable when possible
8. Use appropriate tools for analysis


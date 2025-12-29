# Java Basics

## What is Java?
Java is a high-level, object-oriented programming language developed by Sun Microsystems (now owned by Oracle). It is platform-independent, meaning Java code can run on any device that has a Java Virtual Machine (JVM).

## Key Features
- **Platform Independent**: Write once, run anywhere (WORA)
- **Object-Oriented**: Everything in Java is an object
- **Simple**: Easy to learn and use
- **Secure**: Built-in security features
- **Robust**: Strong memory management and exception handling
- **Multithreaded**: Supports concurrent programming
- **High Performance**: Just-In-Time (JIT) compiler

## Java Program Structure

```java
// Class declaration
public class HelloWorld {
    // Main method - entry point of the program
    public static void main(String[] args) {
        // Print statement
        System.out.println("Hello, World!");
    }
}
```

## Compilation and Execution
1. **Compile**: `javac HelloWorld.java` → creates `HelloWorld.class`
2. **Run**: `java HelloWorld` → executes the bytecode

## Naming Conventions
- **Classes**: PascalCase (e.g., `MyClass`, `StudentRecord`)
- **Methods/Variables**: camelCase (e.g., `calculateTotal()`, `studentName`)
- **Constants**: UPPER_SNAKE_CASE (e.g., `MAX_SIZE`, `PI_VALUE`)
- **Packages**: lowercase (e.g., `com.example.util`)

## Comments
```java
// Single-line comment

/* 
   Multi-line comment
   Can span multiple lines
*/

/**
 * Documentation comment (Javadoc)
 * Used for generating API documentation
 */
```


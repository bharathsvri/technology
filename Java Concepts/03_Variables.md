# Java Variables

## What is a Variable?
A variable is a container that holds data values. It has a name, a type, and a value.

## Variable Declaration Syntax
```java
dataType variableName = value;
```

## Types of Variables

### 1. Local Variables
- Declared inside methods, constructors, or blocks
- Must be initialized before use
- Scope is limited to the block where declared
- No access modifiers allowed

```java
public void method() {
    int localVar = 10; // Local variable
    System.out.println(localVar);
}
```

### 2. Instance Variables (Non-Static Fields)
- Declared in a class, outside any method
- Belongs to an instance of the class
- Each object has its own copy
- Initialized with default values
- Accessible throughout the class

```java
public class Student {
    String name; // Instance variable
    int age;     // Instance variable
    
    public Student(String name, int age) {
        this.name = name;
        this.age = age;
    }
}
```

### 3. Class Variables (Static Fields)
- Declared with `static` keyword
- Belongs to the class, not instances
- Shared by all instances
- Can be accessed without creating an object
- Initialized with default values

```java
public class Counter {
    static int count = 0; // Class variable
    
    public Counter() {
        count++;
    }
}
```

### 4. Parameters
- Variables in method signatures
- Receive values when method is called

```java
public void display(String message) { // 'message' is a parameter
    System.out.println(message);
}
```

## Variable Naming Rules
1. Must start with a letter, underscore (_), or dollar sign ($)
2. Can contain letters, digits, underscores, and dollar signs
3. Cannot be Java keywords
4. Case-sensitive
5. Follow camelCase convention

## Valid and Invalid Examples
```java
// Valid
int age;
String firstName;
double _price;
int $count;
String studentName123;

// Invalid
int 2age;        // Cannot start with digit
String first-name; // Cannot use hyphen
int class;       // 'class' is a keyword
```

## Variable Scope
```java
public class ScopeExample {
    int instanceVar = 10; // Instance variable
    
    public void method() {
        int localVar = 20; // Local variable
        
        if (true) {
            int blockVar = 30; // Block variable
            System.out.println(blockVar); // Accessible here
        }
        // System.out.println(blockVar); // Error: not accessible
    }
}
```

## Final Variables (Constants)
```java
final int MAX_SIZE = 100; // Cannot be changed
final String COMPANY_NAME = "Tech Corp";
```

## Variable Initialization
```java
// Explicit initialization
int x = 10;

// Multiple variables
int a = 1, b = 2, c = 3;

// Default initialization (for instance/class variables)
public class Example {
    int num; // Default value: 0
    String name; // Default value: null
}
```


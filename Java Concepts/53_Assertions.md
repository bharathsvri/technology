# Assertions

## What are Assertions?
Assertions are statements that test assumptions about your program. They help catch bugs during development.

## Basic Syntax
```java
assert condition;
assert condition : errorMessage;
```

## Enabling Assertions
```bash
# Enable assertions
java -ea MyClass
# Or
java -enableassertions MyClass

# Disable assertions (default)
java -da MyClass
# Or
java -disableassertions MyClass

# Enable for specific package
java -ea:com.example... MyClass

# Enable for specific class
java -ea:com.example.MyClass MyClass
```

## Basic Usage
```java
public void setAge(int age) {
    assert age >= 0 : "Age cannot be negative: " + age;
    this.age = age;
}

public void divide(int a, int b) {
    assert b != 0 : "Division by zero";
    int result = a / b;
}
```

## When to Use Assertions
```java
// Internal invariants
private void processData() {
    assert data != null : "Data should not be null";
    // Process data
}

// Control flow invariants
switch (value) {
    case 1:
    case 2:
    case 3:
        break;
    default:
        assert false : "Unexpected value: " + value;
}

// Preconditions
public void method(String param) {
    assert param != null : "Parameter cannot be null";
    // Method body
}

// Postconditions
public int calculate(int a, int b) {
    int result = a + b;
    assert result > 0 : "Result should be positive";
    return result;
}
```

## Assertions vs Exceptions

### Assertions
- For internal errors (bugs)
- Can be disabled in production
- Should not handle user input
- For development and testing

### Exceptions
- For recoverable errors
- Always enabled
- Handle user input
- For production code

```java
// Use assertion for internal check
private void internalMethod(int value) {
    assert value > 0 : "Internal error: value should be positive";
}

// Use exception for user input
public void setValue(int value) {
    if (value <= 0) {
        throw new IllegalArgumentException("Value must be positive");
    }
    this.value = value;
}
```

## Best Practices
1. Don't use assertions for public method parameter validation
2. Don't have side effects in assertions
3. Use assertions for internal invariants
4. Enable assertions during development and testing
5. Disable assertions in production (default)
6. Use descriptive error messages
7. Don't rely on assertions for program logic

## Complete Example
```java
public class AssertionDemo {
    private int[] data;
    
    public AssertionDemo(int size) {
        assert size > 0 : "Size must be positive";
        this.data = new int[size];
    }
    
    public void setValue(int index, int value) {
        assert index >= 0 && index < data.length : 
            "Index out of bounds: " + index;
        assert value >= 0 : "Value cannot be negative";
        data[index] = value;
    }
    
    public int getValue(int index) {
        assert index >= 0 && index < data.length : 
            "Index out of bounds: " + index;
        int value = data[index];
        assert value >= 0 : "Value should be non-negative";
        return value;
    }
    
    public static void main(String[] args) {
        AssertionDemo demo = new AssertionDemo(10);
        demo.setValue(5, 42);
        System.out.println(demo.getValue(5));
    }
}
```


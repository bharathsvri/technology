# Exception Handling

## What is an Exception?
An exception is an event that disrupts the normal flow of program execution. It represents an error condition or unexpected situation.

## Exception Hierarchy
```
Throwable
├── Error (unchecked)
│   ├── OutOfMemoryError
│   ├── StackOverflowError
│   └── ...
└── Exception
    ├── RuntimeException (unchecked)
    │   ├── NullPointerException
    │   ├── ArrayIndexOutOfBoundsException
    │   ├── IllegalArgumentException
    │   └── ...
    └── Checked Exceptions
        ├── IOException
        ├── SQLException
        └── ...
```

## Types of Exceptions

### Checked Exceptions
Must be handled or declared. Compile-time checking.

```java
// Must handle or declare
public void readFile() throws IOException {
    FileReader file = new FileReader("file.txt");
}
```

### Unchecked Exceptions (Runtime Exceptions)
Not required to be handled. Runtime checking.

```java
// No need to handle
int[] arr = new int[5];
int value = arr[10]; // ArrayIndexOutOfBoundsException
```

### Errors
Serious problems that applications should not try to catch.

```java
// OutOfMemoryError, StackOverflowError, etc.
```

## Try-Catch Block

### Basic Syntax
```java
try {
    // Code that may throw exception
} catch (ExceptionType e) {
    // Handle exception
}
```

### Example
```java
try {
    int result = 10 / 0; // ArithmeticException
} catch (ArithmeticException e) {
    System.out.println("Cannot divide by zero: " + e.getMessage());
}
```

## Multiple Catch Blocks
```java
try {
    int[] arr = new int[5];
    int value = arr[10];
    int result = 10 / 0;
} catch (ArrayIndexOutOfBoundsException e) {
    System.out.println("Array index out of bounds");
} catch (ArithmeticException e) {
    System.out.println("Arithmetic error");
} catch (Exception e) {
    System.out.println("General exception: " + e.getMessage());
}
```

## Finally Block
Always executes, regardless of exception.

```java
try {
    // Code that may throw exception
} catch (Exception e) {
    // Handle exception
} finally {
    // Always executes
    System.out.println("Finally block executed");
}
```

## Try-With-Resources (Java 7+)
Automatically closes resources.

```java
try (FileReader file = new FileReader("file.txt");
     BufferedReader reader = new BufferedReader(file)) {
    // Use resources
    String line = reader.readLine();
} catch (IOException e) {
    // Handle exception
}
// Resources automatically closed
```

## Throwing Exceptions

### Throw Keyword
```java
public void setAge(int age) {
    if (age < 0) {
        throw new IllegalArgumentException("Age cannot be negative");
    }
    this.age = age;
}
```

### Throws Keyword
```java
public void readFile() throws IOException {
    FileReader file = new FileReader("file.txt");
    // Code that may throw IOException
}
```

## Custom Exceptions

### Creating Custom Exception
```java
// Checked exception
class InsufficientFundsException extends Exception {
    private double amount;
    
    public InsufficientFundsException(double amount) {
        super("Insufficient funds. Required: " + amount);
        this.amount = amount;
    }
    
    public double getAmount() {
        return amount;
    }
}

// Unchecked exception
class InvalidAgeException extends RuntimeException {
    public InvalidAgeException(String message) {
        super(message);
    }
}
```

### Using Custom Exception
```java
class BankAccount {
    private double balance;
    
    public void withdraw(double amount) throws InsufficientFundsException {
        if (amount > balance) {
            throw new InsufficientFundsException(amount);
        }
        balance -= amount;
    }
}
```

## Common Built-in Exceptions

### NullPointerException
```java
String str = null;
int length = str.length(); // NullPointerException
```

### ArrayIndexOutOfBoundsException
```java
int[] arr = {1, 2, 3};
int value = arr[5]; // ArrayIndexOutOfBoundsException
```

### IllegalArgumentException
```java
public void setAge(int age) {
    if (age < 0) {
        throw new IllegalArgumentException("Age must be positive");
    }
}
```

### NumberFormatException
```java
String str = "abc";
int num = Integer.parseInt(str); // NumberFormatException
```

## Exception Handling Best Practices

### 1. Specific Exception Types
```java
// Good
try {
    // code
} catch (FileNotFoundException e) {
    // handle file not found
} catch (IOException e) {
    // handle other IO errors
}

// Avoid
try {
    // code
} catch (Exception e) {
    // too generic
}
```

### 2. Don't Swallow Exceptions
```java
// Bad
try {
    // code
} catch (Exception e) {
    // Empty catch block - exception is swallowed
}

// Good
try {
    // code
} catch (Exception e) {
    logger.error("Error occurred", e);
    // Handle appropriately
}
```

### 3. Use Finally for Cleanup
```java
Connection conn = null;
try {
    conn = getConnection();
    // use connection
} catch (SQLException e) {
    // handle exception
} finally {
    if (conn != null) {
        conn.close();
    }
}
```

### 4. Try-With-Resources
```java
// Better than manual cleanup
try (Connection conn = getConnection();
     Statement stmt = conn.createStatement()) {
    // use resources
} catch (SQLException e) {
    // handle exception
}
```

## Complete Example
```java
class Calculator {
    public double divide(double a, double b) throws ArithmeticException {
        if (b == 0) {
            throw new ArithmeticException("Cannot divide by zero");
        }
        return a / b;
    }
    
    public int parseNumber(String str) throws NumberFormatException {
        return Integer.parseInt(str);
    }
}

class ExceptionDemo {
    public static void main(String[] args) {
        Calculator calc = new Calculator();
        
        // Handle division
        try {
            double result = calc.divide(10, 0);
            System.out.println("Result: " + result);
        } catch (ArithmeticException e) {
            System.out.println("Error: " + e.getMessage());
        }
        
        // Handle parsing
        try {
            int num = calc.parseNumber("abc");
            System.out.println("Number: " + num);
        } catch (NumberFormatException e) {
            System.out.println("Invalid number format");
        } finally {
            System.out.println("Execution completed");
        }
    }
}
```

## Exception Propagation
```java
public void method1() throws IOException {
    method2();
}

public void method2() throws IOException {
    method3();
}

public void method3() throws IOException {
    throw new IOException("Error occurred");
}
```

## Assertions
Used for debugging and testing.

```java
public void setAge(int age) {
    assert age >= 0 : "Age must be non-negative";
    this.age = age;
}

// Enable assertions: java -ea MyClass
```

## Best Practices Summary
1. Catch specific exceptions, not generic Exception
2. Don't ignore exceptions - handle or log them
3. Use try-with-resources for resource management
4. Throw exceptions early, catch late
5. Create meaningful exception messages
6. Use checked exceptions for recoverable conditions
7. Use unchecked exceptions for programming errors
8. Document exceptions in method signatures


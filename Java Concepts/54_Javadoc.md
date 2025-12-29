# Javadoc

## What is Javadoc?
Javadoc is a documentation generation tool that creates API documentation from Java source code comments.

## Basic Javadoc Comments
```java
/**
 * This is a Javadoc comment.
 * It can span multiple lines.
 */
public class MyClass {
}
```

## Class Documentation
```java
/**
 * Represents a student in the system.
 * 
 * <p>This class stores student information including
 * name, age, and student ID.
 * 
 * @author John Doe
 * @version 1.0
 * @since 1.0
 */
public class Student {
}
```

## Method Documentation
```java
/**
 * Calculates the sum of two integers.
 * 
 * @param a the first integer
 * @param b the second integer
 * @return the sum of a and b
 * @throws IllegalArgumentException if either parameter is negative
 */
public int add(int a, int b) {
    if (a < 0 || b < 0) {
        throw new IllegalArgumentException("Parameters must be non-negative");
    }
    return a + b;
}
```

## Field Documentation
```java
/**
 * The maximum number of students allowed.
 */
private static final int MAX_STUDENTS = 100;

/**
 * The student's name.
 */
private String name;
```

## Common Javadoc Tags

### @param
Documents a method parameter.
```java
/**
 * @param name the student's name
 * @param age the student's age
 */
public Student(String name, int age) {
}
```

### @return
Documents the return value.
```java
/**
 * @return the student's name
 */
public String getName() {
    return name;
}
```

### @throws / @exception
Documents exceptions that may be thrown.
```java
/**
 * @throws IllegalArgumentException if age is negative
 * @throws NullPointerException if name is null
 */
public void setAge(int age) {
    if (age < 0) {
        throw new IllegalArgumentException("Age cannot be negative");
    }
}
```

### @see
References other documentation.
```java
/**
 * @see Student
 * @see #getName()
 * @see java.util.List
 */
public void method() {
}
```

### @since
Indicates when the feature was added.
```java
/**
 * @since 1.5
 */
public void newMethod() {
}
```

### @deprecated
Marks as deprecated.
```java
/**
 * @deprecated Use {@link #newMethod()} instead
 */
@Deprecated
public void oldMethod() {
}
```

### @author
Specifies the author.
```java
/**
 * @author John Doe
 */
public class MyClass {
}
```

### @version
Specifies the version.
```java
/**
 * @version 1.0
 */
public class MyClass {
}
```

## HTML in Javadoc
```java
/**
 * <p>This is a paragraph.</p>
 * 
 * <ul>
 *   <li>Item 1</li>
 *   <li>Item 2</li>
 * </ul>
 * 
 * <pre>
 *   Code example
 * </pre>
 * 
 * <code>inline code</code>
 * 
 * <b>bold</b>
 * <i>italic</i>
 */
```

## Code Examples
```java
/**
 * Calculates the factorial of a number.
 * 
 * <p>Example usage:
 * <pre>{@code
 * int result = factorial(5);
 * // result is 120
 * }</pre>
 * 
 * @param n the number
 * @return the factorial of n
 */
public int factorial(int n) {
    return n <= 1 ? 1 : n * factorial(n - 1);
}
```

## Generating Javadoc
```bash
# Generate Javadoc
javadoc MyClass.java

# Generate with options
javadoc -d docs -author -version MyClass.java

# Generate for package
javadoc -d docs com.example.package

# Generate for multiple packages
javadoc -d docs com.example.package1 com.example.package2
```

## Complete Example
```java
/**
 * Represents a bank account.
 * 
 * <p>This class provides basic banking operations including
 * deposits, withdrawals, and balance inquiries.
 * 
 * @author Jane Smith
 * @version 2.0
 * @since 1.0
 */
public class BankAccount {
    /**
     * The account balance.
     */
    private double balance;
    
    /**
     * Creates a new bank account with zero balance.
     */
    public BankAccount() {
        this.balance = 0.0;
    }
    
    /**
     * Deposits money into the account.
     * 
     * @param amount the amount to deposit (must be positive)
     * @throws IllegalArgumentException if amount is negative or zero
     */
    public void deposit(double amount) {
        if (amount <= 0) {
            throw new IllegalArgumentException("Amount must be positive");
        }
        balance += amount;
    }
    
    /**
     * Withdraws money from the account.
     * 
     * @param amount the amount to withdraw (must be positive)
     * @return true if withdrawal was successful, false if insufficient funds
     * @throws IllegalArgumentException if amount is negative or zero
     */
    public boolean withdraw(double amount) {
        if (amount <= 0) {
            throw new IllegalArgumentException("Amount must be positive");
        }
        if (balance >= amount) {
            balance -= amount;
            return true;
        }
        return false;
    }
    
    /**
     * Gets the current account balance.
     * 
     * @return the current balance
     */
    public double getBalance() {
        return balance;
    }
}
```

## Best Practices
1. Document all public classes and methods
2. Write clear, concise descriptions
3. Use @param, @return, @throws consistently
4. Include code examples for complex methods
5. Keep documentation up to date
6. Use HTML formatting sparingly
7. Generate and review Javadoc regularly
8. Follow standard Javadoc conventions


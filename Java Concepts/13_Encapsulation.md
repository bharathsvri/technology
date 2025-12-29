# Encapsulation

## What is Encapsulation?
Encapsulation is the bundling of data (fields) and methods that operate on that data within a single unit (class). It restricts direct access to some components and can prevent accidental modification of data.

## Principles of Encapsulation
1. **Data Hiding**: Hide internal implementation details
2. **Access Control**: Use access modifiers to control visibility
3. **Controlled Access**: Provide getters/setters for controlled access

## Access Modifiers

### Private
Fields and methods accessible only within the class.

```java
public class BankAccount {
    private double balance; // Hidden from outside
    
    private void validateAmount(double amount) {
        // Internal validation logic
    }
}
```

### Public
Accessible from anywhere.

```java
public class BankAccount {
    public void deposit(double amount) {
        // Public method for external use
    }
}
```

### Protected
Accessible within package and subclasses.

```java
public class BankAccount {
    protected String accountType; // Accessible in subclasses
}
```

### Default (Package-Private)
Accessible within the same package.

```java
class BankAccount {
    String accountNumber; // Default access
}
```

## Getter and Setter Methods

### Basic Example
```java
public class Person {
    private String name;
    private int age;
    
    // Getter methods
    public String getName() {
        return name;
    }
    
    public int getAge() {
        return age;
    }
    
    // Setter methods
    public void setName(String name) {
        this.name = name;
    }
    
    public void setAge(int age) {
        if (age > 0 && age < 150) {
            this.age = age;
        }
    }
}
```

### With Validation
```java
public class BankAccount {
    private double balance;
    
    public double getBalance() {
        return balance;
    }
    
    public void deposit(double amount) {
        if (amount > 0) {
            balance += amount;
        } else {
            System.out.println("Invalid amount");
        }
    }
    
    public boolean withdraw(double amount) {
        if (amount > 0 && amount <= balance) {
            balance -= amount;
            return true;
        }
        return false;
    }
}
```

## Complete Encapsulation Example
```java
public class Student {
    // Private fields - data hiding
    private String name;
    private int age;
    private String studentId;
    private double gpa;
    
    // Constructor
    public Student(String name, int age, String studentId) {
        this.name = name;
        setAge(age); // Use setter for validation
        this.studentId = studentId;
        this.gpa = 0.0;
    }
    
    // Getter methods
    public String getName() {
        return name;
    }
    
    public int getAge() {
        return age;
    }
    
    public String getStudentId() {
        return studentId;
    }
    
    public double getGpa() {
        return gpa;
    }
    
    // Setter methods with validation
    public void setName(String name) {
        if (name != null && !name.trim().isEmpty()) {
            this.name = name;
        }
    }
    
    public void setAge(int age) {
        if (age >= 0 && age <= 120) {
            this.age = age;
        } else {
            throw new IllegalArgumentException("Invalid age");
        }
    }
    
    // Read-only field (no setter for studentId)
    // GPA can only be updated through methods
    
    public void updateGpa(double newGpa) {
        if (newGpa >= 0.0 && newGpa <= 4.0) {
            this.gpa = newGpa;
        } else {
            throw new IllegalArgumentException("GPA must be between 0.0 and 4.0");
        }
    }
    
    // Public method to display information
    public void displayInfo() {
        System.out.println("Student ID: " + studentId);
        System.out.println("Name: " + name);
        System.out.println("Age: " + age);
        System.out.println("GPA: " + gpa);
    }
}
```

## Benefits of Encapsulation
1. **Data Protection**: Prevents unauthorized access
2. **Flexibility**: Can change internal implementation without affecting users
3. **Validation**: Can validate data before setting
4. **Maintainability**: Easier to maintain and debug
5. **Security**: Protects sensitive data

## Read-Only and Write-Only Properties

### Read-Only (No Setter)
```java
public class Circle {
    private final double radius;
    
    public Circle(double radius) {
        this.radius = radius;
    }
    
    public double getRadius() {
        return radius;
    }
    
    // No setter - radius is read-only after construction
}
```

### Write-Only (No Getter)
```java
public class PasswordManager {
    private String password;
    
    public void setPassword(String password) {
        // Hash and store password
        this.password = hashPassword(password);
    }
    
    // No getter - password cannot be retrieved
    public boolean verifyPassword(String input) {
        return hashPassword(input).equals(password);
    }
}
```

## Best Practices
1. Make fields private by default
2. Provide getters for fields that need to be read
3. Provide setters with validation
4. Use meaningful method names
5. Don't expose internal implementation details
6. Use final for immutable fields


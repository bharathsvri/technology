# Interfaces

## What is an Interface?
An interface is a reference type in Java that contains abstract methods, constants, default methods, and static methods. It defines a contract that implementing classes must follow.

## Basic Interface Syntax
```java
interface InterfaceName {
    // Constants (implicitly public static final)
    int CONSTANT = 100;
    
    // Abstract methods (implicitly public abstract)
    void methodName();
    
    // Default methods (Java 8+)
    default void defaultMethod() {
        // implementation
    }
    
    // Static methods (Java 8+)
    static void staticMethod() {
        // implementation
    }
}
```

## Simple Interface Example
```java
interface Drawable {
    void draw();
}

class Circle implements Drawable {
    @Override
    public void draw() {
        System.out.println("Drawing a circle");
    }
}

class Rectangle implements Drawable {
    @Override
    public void draw() {
        System.out.println("Drawing a rectangle");
    }
}
```

## Interface Constants
All fields in interface are implicitly `public static final`.

```java
interface Constants {
    int MAX_SIZE = 100;
    String DEFAULT_NAME = "Unknown";
    double PI = 3.14159;
}

// Usage
int max = Constants.MAX_SIZE;
```

## Multiple Interface Implementation
A class can implement multiple interfaces.

```java
interface Flyable {
    void fly();
}

interface Swimmable {
    void swim();
}

interface Walkable {
    void walk();
}

class Duck implements Flyable, Swimmable, Walkable {
    @Override
    public void fly() {
        System.out.println("Duck is flying");
    }
    
    @Override
    public void swim() {
        System.out.println("Duck is swimming");
    }
    
    @Override
    public void walk() {
        System.out.println("Duck is walking");
    }
}
```

## Interface Inheritance
Interfaces can extend other interfaces.

```java
interface Animal {
    void eat();
    void sleep();
}

interface Flyable {
    void fly();
}

interface Bird extends Animal, Flyable {
    void chirp();
}

class Sparrow implements Bird {
    @Override
    public void eat() {
        System.out.println("Sparrow is eating");
    }
    
    @Override
    public void sleep() {
        System.out.println("Sparrow is sleeping");
    }
    
    @Override
    public void fly() {
        System.out.println("Sparrow is flying");
    }
    
    @Override
    public void chirp() {
        System.out.println("Sparrow is chirping");
    }
}
```

## Default Methods (Java 8+)
Provide default implementation that can be overridden.

```java
interface Calculator {
    int add(int a, int b);
    
    default int subtract(int a, int b) {
        return a - b;
    }
    
    default int multiply(int a, int b) {
        return a * b;
    }
}

class BasicCalculator implements Calculator {
    @Override
    public int add(int a, int b) {
        return a + b;
    }
    
    // Can use default methods or override them
}
```

## Static Methods (Java 8+)
Belong to interface, called using interface name.

```java
interface MathUtils {
    static int max(int a, int b) {
        return a > b ? a : b;
    }
    
    static int min(int a, int b) {
        return a < b ? a : b;
    }
}

// Usage
int max = MathUtils.max(5, 3);
int min = MathUtils.min(5, 3);
```

## Private Methods (Java 9+)
Helper methods used within interface.

```java
interface Calculator {
    default int complexCalculation(int a, int b) {
        return helper1(a) + helper2(b);
    }
    
    private int helper1(int x) {
        return x * 2;
    }
    
    private static int helper2(int x) {
        return x * 3;
    }
}
```

## Functional Interfaces (Java 8+)
Interface with exactly one abstract method. Can use lambda expressions.

```java
@FunctionalInterface
interface MathOperation {
    int operate(int a, int b);
}

// Usage with lambda
MathOperation add = (a, b) -> a + b;
MathOperation multiply = (a, b) -> a * b;

int result = add.operate(5, 3); // 8
```

## Marker Interfaces
Interfaces with no methods. Used to mark classes.

```java
interface Serializable {
    // No methods - just a marker
}

class MyClass implements Serializable {
    // Marked as serializable
}
```

## Common Java Interfaces

### Comparable Interface
```java
class Student implements Comparable<Student> {
    private String name;
    private int age;
    
    @Override
    public int compareTo(Student other) {
        return this.name.compareTo(other.name);
    }
}
```

### Cloneable Interface
```java
class Person implements Cloneable {
    private String name;
    
    @Override
    public Object clone() throws CloneNotSupportedException {
        return super.clone();
    }
}
```

### Runnable Interface
```java
class MyTask implements Runnable {
    @Override
    public void run() {
        System.out.println("Task is running");
    }
}

// Usage
Thread thread = new Thread(new MyTask());
thread.start();
```

## Complete Example
```java
// Payment interface
interface Payment {
    void processPayment(double amount);
    boolean validatePayment();
    
    default void refund(double amount) {
        System.out.println("Refunding: " + amount);
    }
    
    static Payment getPaymentMethod(String type) {
        if (type.equals("credit")) {
            return new CreditCardPayment();
        } else if (type.equals("paypal")) {
            return new PayPalPayment();
        }
        return null;
    }
}

// Credit card implementation
class CreditCardPayment implements Payment {
    private String cardNumber;
    
    public CreditCardPayment(String cardNumber) {
        this.cardNumber = cardNumber;
    }
    
    @Override
    public void processPayment(double amount) {
        System.out.println("Processing credit card payment: " + amount);
    }
    
    @Override
    public boolean validatePayment() {
        return cardNumber != null && cardNumber.length() == 16;
    }
}

// PayPal implementation
class PayPalPayment implements Payment {
    private String email;
    
    public PayPalPayment(String email) {
        this.email = email;
    }
    
    @Override
    public void processPayment(double amount) {
        System.out.println("Processing PayPal payment: " + amount);
    }
    
    @Override
    public boolean validatePayment() {
        return email != null && email.contains("@");
    }
}

// Usage
public class PaymentDemo {
    public static void main(String[] args) {
        Payment payment1 = new CreditCardPayment("1234567890123456");
        Payment payment2 = new PayPalPayment("user@example.com");
        
        payment1.processPayment(100.0);
        payment2.processPayment(200.0);
        
        // Using static method
        Payment payment = Payment.getPaymentMethod("credit");
    }
}
```

## Benefits of Interfaces
1. **Abstraction**: Hide implementation details
2. **Multiple Inheritance**: Implement multiple interfaces
3. **Loose Coupling**: Depend on contracts, not implementations
4. **Polymorphism**: Treat different implementations uniformly
5. **API Design**: Define clear contracts for APIs

## Best Practices
1. Use interfaces for contracts
2. Keep interfaces focused (Single Responsibility)
3. Use default methods for backward compatibility
4. Use @FunctionalInterface annotation for functional interfaces
5. Prefer composition over inheritance when possible


# Abstraction

## What is Abstraction?
Abstraction is the process of hiding implementation details and showing only essential features. It focuses on what an object does rather than how it does it.

## Types of Abstraction

### 1. Abstract Classes
Classes that cannot be instantiated and may contain abstract methods.

### 2. Interfaces
Contracts that define what methods a class must implement.

## Abstract Classes

### Basic Syntax
```java
abstract class Animal {
    // Abstract method (no implementation)
    abstract void makeSound();
    
    // Concrete method (has implementation)
    void sleep() {
        System.out.println("Animal is sleeping");
    }
}
```

### Abstract Class Example
```java
abstract class Shape {
    protected String color;
    
    // Abstract methods - must be implemented by subclasses
    abstract double calculateArea();
    abstract double calculatePerimeter();
    
    // Concrete method - shared by all subclasses
    public void setColor(String color) {
        this.color = color;
    }
    
    public String getColor() {
        return color;
    }
}

// Concrete subclass
class Circle extends Shape {
    private double radius;
    
    public Circle(double radius) {
        this.radius = radius;
    }
    
    @Override
    double calculateArea() {
        return Math.PI * radius * radius;
    }
    
    @Override
    double calculatePerimeter() {
        return 2 * Math.PI * radius;
    }
}

// Concrete subclass
class Rectangle extends Shape {
    private double width;
    private double height;
    
    public Rectangle(double width, double height) {
        this.width = width;
        this.height = height;
    }
    
    @Override
    double calculateArea() {
        return width * height;
    }
    
    @Override
    double calculatePerimeter() {
        return 2 * (width + height);
    }
}
```

### Rules for Abstract Classes
1. Cannot be instantiated
2. Can have abstract and concrete methods
3. Can have fields, constructors, static methods
4. Subclass must implement all abstract methods
5. Can have final methods

## Interfaces

### Basic Syntax
```java
interface Drawable {
    void draw(); // Implicitly public and abstract
}
```

### Interface Example
```java
interface Vehicle {
    void start();
    void stop();
    void accelerate();
}

class Car implements Vehicle {
    @Override
    public void start() {
        System.out.println("Car started");
    }
    
    @Override
    public void stop() {
        System.out.println("Car stopped");
    }
    
    @Override
    public void accelerate() {
        System.out.println("Car accelerating");
    }
}
```

### Multiple Interface Implementation
```java
interface Flyable {
    void fly();
}

interface Swimmable {
    void swim();
}

class Duck implements Flyable, Swimmable {
    @Override
    public void fly() {
        System.out.println("Duck is flying");
    }
    
    @Override
    public void swim() {
        System.out.println("Duck is swimming");
    }
}
```

### Interface Features (Java 8+)

#### Default Methods
```java
interface Calculator {
    int add(int a, int b);
    
    // Default method - has implementation
    default int multiply(int a, int b) {
        return a * b;
    }
}
```

#### Static Methods
```java
interface MathUtils {
    static int max(int a, int b) {
        return a > b ? a : b;
    }
}

// Usage
int result = MathUtils.max(5, 3);
```

#### Private Methods (Java 9+)
```java
interface Calculator {
    default int complexCalculation(int a, int b) {
        return helperMethod(a) + helperMethod(b);
    }
    
    private int helperMethod(int x) {
        return x * 2;
    }
}
```

## Abstract Class vs Interface

| Feature | Abstract Class | Interface |
|---------|---------------|-----------|
| Instantiation | Cannot be instantiated | Cannot be instantiated |
| Methods | Can have abstract and concrete | All methods abstract (until Java 8) |
| Fields | Can have any fields | Only constants (public static final) |
| Multiple Inheritance | Single inheritance | Multiple implementation |
| Constructors | Can have constructors | Cannot have constructors |
| Access Modifiers | Any access modifier | Public by default |

## When to Use Abstract Class
- Share code among related classes
- Have fields that subclasses need
- Need constructors
- Want to provide default implementations

## When to Use Interface
- Define contract for unrelated classes
- Need multiple inheritance
- Want to achieve loose coupling
- Creating API contracts

## Complete Example
```java
// Abstract class
abstract class Employee {
    protected String name;
    protected double baseSalary;
    
    public Employee(String name, double baseSalary) {
        this.name = name;
        this.baseSalary = baseSalary;
    }
    
    // Abstract method - must be implemented
    abstract double calculateSalary();
    
    // Concrete method - shared implementation
    public void displayInfo() {
        System.out.println("Name: " + name);
        System.out.println("Salary: " + calculateSalary());
    }
}

// Interface
interface BonusEligible {
    double calculateBonus();
}

// Concrete class implementing both
class Manager extends Employee implements BonusEligible {
    private double bonusPercentage;
    
    public Manager(String name, double baseSalary, double bonusPercentage) {
        super(name, baseSalary);
        this.bonusPercentage = bonusPercentage;
    }
    
    @Override
    double calculateSalary() {
        return baseSalary + calculateBonus();
    }
    
    @Override
    public double calculateBonus() {
        return baseSalary * bonusPercentage / 100;
    }
}
```

## Benefits of Abstraction
1. **Simplifies Complexity**: Hides implementation details
2. **Flexibility**: Easy to change implementation
3. **Code Reusability**: Share common code
4. **Security**: Prevents direct access to internal details
5. **Maintainability**: Easier to maintain and extend


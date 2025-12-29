# Classes and Objects

## What is a Class?
A class is a blueprint or template for creating objects. It defines the properties (fields) and behaviors (methods) that objects of that type will have.

## What is an Object?
An object is an instance of a class. It has state (stored in fields) and behavior (defined by methods).

## Basic Class Structure
```java
public class Car {
    // Fields (attributes/properties)
    private String brand;
    private String color;
    private int speed;
    
    // Constructor
    public Car(String brand, String color) {
        this.brand = brand;
        this.color = color;
        this.speed = 0;
    }
    
    // Methods (behaviors)
    public void accelerate() {
        speed += 10;
    }
    
    public void brake() {
        if (speed > 0) {
            speed -= 10;
        }
    }
    
    // Getter methods
    public String getBrand() {
        return brand;
    }
    
    public int getSpeed() {
        return speed;
    }
}
```

## Creating Objects
```java
// Create object using new keyword
Car myCar = new Car("Toyota", "Red");

// Access methods
myCar.accelerate();
System.out.println(myCar.getSpeed()); // 10

// Create multiple objects
Car car1 = new Car("Honda", "Blue");
Car car2 = new Car("Ford", "Black");
```

## Constructors

### Default Constructor
```java
public class Student {
    private String name;
    
    // Default constructor (provided by Java if none defined)
    public Student() {
        // Initialize with default values
    }
}
```

### Parameterized Constructor
```java
public class Student {
    private String name;
    private int age;
    
    public Student(String name, int age) {
        this.name = name;
        this.age = age;
    }
}
```

### Constructor Overloading
```java
public class Student {
    private String name;
    private int age;
    
    public Student() {
        this.name = "Unknown";
        this.age = 0;
    }
    
    public Student(String name) {
        this.name = name;
        this.age = 0;
    }
    
    public Student(String name, int age) {
        this.name = name;
        this.age = age;
    }
}
```

## This Keyword
Refers to the current object instance.

```java
public class Person {
    private String name;
    
    public Person(String name) {
        this.name = name; // 'this' refers to current object
    }
    
    public void setName(String name) {
        this.name = name; // Distinguish parameter from field
    }
}
```

## Access Modifiers

### Public
Accessible from anywhere.

```java
public class MyClass {
    public int publicField;
    public void publicMethod() { }
}
```

### Private
Accessible only within the class.

```java
public class MyClass {
    private int privateField;
    private void privateMethod() { }
}
```

### Protected
Accessible within package and subclasses.

```java
public class MyClass {
    protected int protectedField;
    protected void protectedMethod() { }
}
```

### Default (Package-Private)
Accessible within the same package.

```java
class MyClass {
    int defaultField;
    void defaultMethod() { }
}
```

## Getter and Setter Methods
Encapsulation - control access to fields.

```java
public class BankAccount {
    private double balance;
    
    // Getter
    public double getBalance() {
        return balance;
    }
    
    // Setter with validation
    public void setBalance(double balance) {
        if (balance >= 0) {
            this.balance = balance;
        }
    }
}
```

## Static Members

### Static Variables
Belong to the class, not instances.

```java
public class Counter {
    static int count = 0; // Shared by all instances
    
    public Counter() {
        count++;
    }
    
    public static int getCount() {
        return count;
    }
}
```

### Static Methods
Called using class name, cannot access instance variables.

```java
public class MathUtils {
    public static int add(int a, int b) {
        return a + b;
    }
}

// Usage
int result = MathUtils.add(5, 3);
```

## Object Comparison

### == Operator
Compares references, not values.

```java
String str1 = new String("Hello");
String str2 = new String("Hello");
System.out.println(str1 == str2); // false (different references)
```

### equals() Method
Compares values.

```java
String str1 = new String("Hello");
String str2 = new String("Hello");
System.out.println(str1.equals(str2)); // true (same content)
```

## Complete Example
```java
public class Book {
    // Fields
    private String title;
    private String author;
    private int pages;
    private boolean isRead;
    
    // Constructor
    public Book(String title, String author, int pages) {
        this.title = title;
        this.author = author;
        this.pages = pages;
        this.isRead = false;
    }
    
    // Getters
    public String getTitle() {
        return title;
    }
    
    public String getAuthor() {
        return author;
    }
    
    // Methods
    public void markAsRead() {
        this.isRead = true;
    }
    
    public boolean isRead() {
        return isRead;
    }
    
    public String getInfo() {
        return title + " by " + author + " (" + pages + " pages)";
    }
    
    // Main method for testing
    public static void main(String[] args) {
        Book book = new Book("Java Guide", "John Doe", 300);
        System.out.println(book.getInfo());
        book.markAsRead();
        System.out.println("Read: " + book.isRead());
    }
}
```

## Best Practices
1. **Encapsulation**: Make fields private, provide public getters/setters
2. **Meaningful Names**: Use descriptive class and method names
3. **Single Responsibility**: Each class should have one clear purpose
4. **Initialize Fields**: Set default values in constructors
5. **Use this**: When field and parameter names conflict
6. **Documentation**: Add comments for complex logic


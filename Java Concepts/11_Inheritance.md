# Inheritance

## What is Inheritance?
Inheritance is a mechanism where a new class (child/subclass) acquires the properties and behaviors of an existing class (parent/superclass). It promotes code reusability and establishes an "is-a" relationship.

## Basic Inheritance Syntax
```java
// Parent class (Superclass)
class Animal {
    String name;
    
    public void eat() {
        System.out.println(name + " is eating");
    }
    
    public void sleep() {
        System.out.println(name + " is sleeping");
    }
}

// Child class (Subclass)
class Dog extends Animal {
    public void bark() {
        System.out.println(name + " is barking");
    }
}
```

## Using Inheritance
```java
Dog dog = new Dog();
dog.name = "Buddy";
dog.eat();    // Inherited from Animal
dog.sleep();  // Inherited from Animal
dog.bark();   // Defined in Dog
```

## Types of Inheritance

### Single Inheritance
One class extends another class.

```java
class Vehicle {
    int speed;
}

class Car extends Vehicle {
    int wheels = 4;
}
```

### Multilevel Inheritance
Chain of inheritance.

```java
class Animal {
    void eat() { }
}

class Mammal extends Animal {
    void walk() { }
}

class Dog extends Mammal {
    void bark() { }
}
```

### Hierarchical Inheritance
Multiple classes extend one class.

```java
class Animal {
    void eat() { }
}

class Dog extends Animal {
    void bark() { }
}

class Cat extends Animal {
    void meow() { }
}
```

## Super Keyword

### Accessing Parent Class Members
```java
class Parent {
    String name = "Parent";
    
    void display() {
        System.out.println("Parent method");
    }
}

class Child extends Parent {
    String name = "Child";
    
    void display() {
        super.display(); // Call parent method
        System.out.println("Child method");
        System.out.println(super.name); // Access parent field
    }
}
```

### Calling Parent Constructor
```java
class Parent {
    Parent() {
        System.out.println("Parent constructor");
    }
    
    Parent(String name) {
        System.out.println("Parent: " + name);
    }
}

class Child extends Parent {
    Child() {
        super(); // Call parent default constructor
        System.out.println("Child constructor");
    }
    
    Child(String name) {
        super(name); // Call parent parameterized constructor
        System.out.println("Child: " + name);
    }
}
```

## Method Overriding
Subclass provides specific implementation of parent method.

```java
class Animal {
    void makeSound() {
        System.out.println("Animal makes sound");
    }
}

class Dog extends Animal {
    @Override
    void makeSound() {
        System.out.println("Dog barks");
    }
}

class Cat extends Animal {
    @Override
    void makeSound() {
        System.out.println("Cat meows");
    }
}
```

## Rules for Method Overriding
1. Method name and parameters must be same
2. Return type must be same or covariant (subtype)
3. Access modifier cannot be more restrictive
4. Cannot override static, final, or private methods
5. Use `@Override` annotation

## Final Keyword with Inheritance

### Final Class
Cannot be inherited.

```java
final class FinalClass {
    // Cannot be extended
}

// class Child extends FinalClass { } // Error
```

### Final Method
Cannot be overridden.

```java
class Parent {
    final void method() {
        // Cannot be overridden
    }
}

class Child extends Parent {
    // void method() { } // Error
}
```

## Access Modifiers in Inheritance
```java
class Parent {
    public int publicField;
    protected int protectedField;
    int defaultField;
    private int privateField; // Not inherited
}
```

## Constructor in Inheritance
```java
class Parent {
    Parent() {
        System.out.println("Parent constructor");
    }
}

class Child extends Parent {
    Child() {
        // super() is called implicitly
        System.out.println("Child constructor");
    }
}
```

## Complete Example
```java
// Base class
class Employee {
    protected String name;
    protected double salary;
    
    public Employee(String name, double salary) {
        this.name = name;
        this.salary = salary;
    }
    
    public void displayInfo() {
        System.out.println("Name: " + name + ", Salary: " + salary);
    }
    
    public double calculateBonus() {
        return salary * 0.1; // 10% bonus
    }
}

// Derived class
class Manager extends Employee {
    private String department;
    
    public Manager(String name, double salary, String department) {
        super(name, salary);
        this.department = department;
    }
    
    @Override
    public double calculateBonus() {
        return salary * 0.2; // 20% bonus for managers
    }
    
    @Override
    public void displayInfo() {
        super.displayInfo();
        System.out.println("Department: " + department);
    }
}

// Usage
public class InheritanceDemo {
    public static void main(String[] args) {
        Employee emp = new Employee("John", 50000);
        emp.displayInfo();
        System.out.println("Bonus: " + emp.calculateBonus());
        
        Manager mgr = new Manager("Jane", 80000, "IT");
        mgr.displayInfo();
        System.out.println("Bonus: " + mgr.calculateBonus());
    }
}
```

## Benefits of Inheritance
1. **Code Reusability**: Reuse parent class code
2. **Polymorphism**: Enable runtime polymorphism
3. **Maintainability**: Changes in parent affect all children
4. **Organization**: Logical hierarchy of classes

## When to Use Inheritance
- "Is-a" relationship exists
- Need to share common code
- Need polymorphism
- Parent class is stable and well-designed

## Composition vs Inheritance
```java
// Inheritance (is-a)
class Car extends Vehicle { }

// Composition (has-a) - Often preferred
class Car {
    private Engine engine; // Car has an Engine
    private Wheel[] wheels; // Car has Wheels
}
```


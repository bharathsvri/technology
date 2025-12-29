# Polymorphism

## What is Polymorphism?
Polymorphism means "many forms". It allows objects of different types to be treated as objects of a common base type. In Java, polymorphism enables one interface to be used for a general class of actions.

## Types of Polymorphism

### 1. Compile-Time Polymorphism (Method Overloading)
Multiple methods with same name but different parameters.

```java
class Calculator {
    // Method overloading
    public int add(int a, int b) {
        return a + b;
    }
    
    public int add(int a, int b, int c) {
        return a + b + c;
    }
    
    public double add(double a, double b) {
        return a + b;
    }
}

// Usage
Calculator calc = new Calculator();
calc.add(5, 3);        // Calls first method
calc.add(5, 3, 2);     // Calls second method
calc.add(5.5, 3.2);    // Calls third method
```

### 2. Runtime Polymorphism (Method Overriding)
Method call resolved at runtime based on object type.

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

// Runtime polymorphism
Animal animal1 = new Dog();
Animal animal2 = new Cat();

animal1.makeSound(); // "Dog barks" - resolved at runtime
animal2.makeSound(); // "Cat meows" - resolved at runtime
```

## Upcasting and Downcasting

### Upcasting (Implicit)
Treating subclass object as superclass type.

```java
Animal animal = new Dog(); // Upcasting
animal.makeSound(); // Calls Dog's makeSound()
```

### Downcasting (Explicit)
Treating superclass object as subclass type.

```java
Animal animal = new Dog();
Dog dog = (Dog) animal; // Downcasting
dog.bark(); // Can now call Dog-specific methods
```

### instanceof Operator
Check object type before downcasting.

```java
Animal animal = new Dog();

if (animal instanceof Dog) {
    Dog dog = (Dog) animal;
    dog.bark();
}
```

## Virtual Method Invocation
Java uses virtual method invocation - method call is resolved at runtime based on actual object type, not reference type.

```java
class Shape {
    void draw() {
        System.out.println("Drawing shape");
    }
}

class Circle extends Shape {
    @Override
    void draw() {
        System.out.println("Drawing circle");
    }
}

class Rectangle extends Shape {
    @Override
    void draw() {
        System.out.println("Drawing rectangle");
    }
}

// Virtual method invocation
Shape shape1 = new Circle();
Shape shape2 = new Rectangle();

shape1.draw(); // "Drawing circle" - runtime resolution
shape2.draw(); // "Drawing rectangle" - runtime resolution
```

## Polymorphism with Arrays
```java
Animal[] animals = new Animal[3];
animals[0] = new Dog();
animals[1] = new Cat();
animals[2] = new Dog();

for (Animal animal : animals) {
    animal.makeSound(); // Polymorphic call
}
```

## Polymorphism with Method Parameters
```java
class Zoo {
    public void feedAnimal(Animal animal) {
        animal.makeSound(); // Works with any Animal subclass
    }
}

// Usage
Zoo zoo = new Zoo();
zoo.feedAnimal(new Dog());
zoo.feedAnimal(new Cat());
```

## Polymorphism with Return Types
```java
class AnimalFactory {
    public Animal getAnimal(String type) {
        if (type.equals("dog")) {
            return new Dog();
        } else if (type.equals("cat")) {
            return new Cat();
        }
        return new Animal();
    }
}

// Usage
AnimalFactory factory = new AnimalFactory();
Animal animal = factory.getAnimal("dog");
animal.makeSound(); // Calls Dog's makeSound()
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
    
    public double calculatePay() {
        return salary;
    }
    
    public void displayInfo() {
        System.out.println("Employee: " + name);
    }
}

// Subclass 1
class FullTimeEmployee extends Employee {
    private double bonus;
    
    public FullTimeEmployee(String name, double salary, double bonus) {
        super(name, salary);
        this.bonus = bonus;
    }
    
    @Override
    public double calculatePay() {
        return salary + bonus;
    }
    
    @Override
    public void displayInfo() {
        System.out.println("Full-time Employee: " + name);
    }
}

// Subclass 2
class PartTimeEmployee extends Employee {
    private int hours;
    private double hourlyRate;
    
    public PartTimeEmployee(String name, int hours, double hourlyRate) {
        super(name, 0);
        this.hours = hours;
        this.hourlyRate = hourlyRate;
    }
    
    @Override
    public double calculatePay() {
        return hours * hourlyRate;
    }
    
    @Override
    public void displayInfo() {
        System.out.println("Part-time Employee: " + name);
    }
}

// Polymorphism in action
public class PolymorphismDemo {
    public static void main(String[] args) {
        Employee[] employees = {
            new FullTimeEmployee("John", 50000, 5000),
            new PartTimeEmployee("Jane", 20, 25),
            new Employee("Bob", 30000)
        };
        
        // Polymorphic method calls
        for (Employee emp : employees) {
            emp.displayInfo();
            System.out.println("Pay: $" + emp.calculatePay());
            System.out.println();
        }
    }
}
```

## Benefits of Polymorphism
1. **Flexibility**: Code works with different object types
2. **Extensibility**: Easy to add new subclasses
3. **Maintainability**: Changes in one place affect all
4. **Code Reusability**: Write once, use with multiple types

## Rules for Runtime Polymorphism
1. Method must be overridden (not hidden)
2. Method must be called through reference variable
3. Reference and object must have inheritance relationship
4. Overridden method must have same signature

## Method Hiding (Static Methods)
Static methods cannot be overridden, only hidden.

```java
class Parent {
    static void method() {
        System.out.println("Parent method");
    }
}

class Child extends Parent {
    static void method() {
        System.out.println("Child method");
    }
}

// Method hiding - resolved at compile time
Parent obj = new Child();
obj.method(); // "Parent method" - uses reference type
```

## Best Practices
1. Use `@Override` annotation for clarity
2. Design base classes carefully for extensibility
3. Use polymorphism for code flexibility
4. Avoid downcasting when possible
5. Use interfaces for maximum flexibility


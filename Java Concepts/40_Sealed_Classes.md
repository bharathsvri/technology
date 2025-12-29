# Sealed Classes (Java 17+)

## What are Sealed Classes?
Sealed classes restrict which classes can extend or implement them. They provide controlled inheritance.

## Basic Sealed Class
```java
sealed class Shape permits Circle, Rectangle, Triangle {
    abstract double area();
}

final class Circle extends Shape {
    private final double radius;
    
    public Circle(double radius) {
        this.radius = radius;
    }
    
    @Override
    double area() {
        return Math.PI * radius * radius;
    }
}

final class Rectangle extends Shape {
    private final double width;
    private final double height;
    
    public Rectangle(double width, double height) {
        this.width = width;
        this.height = height;
    }
    
    @Override
    double area() {
        return width * height;
    }
}

final class Triangle extends Shape {
    private final double base;
    private final double height;
    
    public Triangle(double base, double height) {
        this.base = base;
        this.height = height;
    }
    
    @Override
    double area() {
        return 0.5 * base * height;
    }
}
```

## Sealed Interface
```java
sealed interface PaymentMethod permits CreditCard, PayPal, BankTransfer {
    void pay(double amount);
}

final class CreditCard implements PaymentMethod {
    @Override
    public void pay(double amount) {
        System.out.println("Paying $" + amount + " with Credit Card");
    }
}

final class PayPal implements PaymentMethod {
    @Override
    public void pay(double amount) {
        System.out.println("Paying $" + amount + " with PayPal");
    }
}

final class BankTransfer implements PaymentMethod {
    @Override
    public void pay(double amount) {
        System.out.println("Paying $" + amount + " with Bank Transfer");
    }
}
```

## Non-Sealed Subclasses
```java
sealed class Animal permits Mammal, Bird {
    abstract void makeSound();
}

non-sealed class Mammal extends Animal {
    @Override
    void makeSound() {
        System.out.println("Mammal sound");
    }
}

// Mammal can be extended by any class
class Dog extends Mammal {
    @Override
    void makeSound() {
        System.out.println("Woof");
    }
}

final class Bird extends Animal {
    @Override
    void makeSound() {
        System.out.println("Tweet");
    }
}
```

## Sealed Classes with Pattern Matching
```java
sealed class Expr permits ConstantExpr, AddExpr, MulExpr {
}

record ConstantExpr(int i) implements Expr {}
record AddExpr(Expr left, Expr right) implements Expr {}
record MulExpr(Expr left, Expr right) implements Expr {}

// Pattern matching with sealed classes
int eval(Expr e) {
    return switch (e) {
        case ConstantExpr(int i) -> i;
        case AddExpr(Expr left, Expr right) -> eval(left) + eval(right);
        case MulExpr(Expr left, Expr right) -> eval(left) * eval(right);
    };
}

// Usage
Expr expr = new AddExpr(
    new ConstantExpr(5),
    new MulExpr(new ConstantExpr(2), new ConstantExpr(3))
);
int result = eval(expr); // 11
```

## Sealed Classes in Collections
```java
sealed interface Result<T> permits Success, Failure {
}

record Success<T>(T value) implements Result<T> {}
record Failure<T>(String error) implements Result<T> {}

// Usage
List<Result<String>> results = Arrays.asList(
    new Success<>("Data"),
    new Failure<>("Error occurred"),
    new Success<>("More data")
);

for (Result<String> result : results) {
    switch (result) {
        case Success<String>(String value) -> 
            System.out.println("Success: " + value);
        case Failure<String>(String error) -> 
            System.out.println("Failure: " + error);
    }
}
```

## Benefits
1. **Controlled Inheritance**: Limit who can extend your class
2. **Exhaustive Pattern Matching**: Compiler ensures all cases covered
3. **Better API Design**: Clear contract for subclasses
4. **Type Safety**: Prevents unauthorized extensions
5. **Documentation**: Makes inheritance hierarchy explicit

## Best Practices
1. Use sealed classes for fixed hierarchies
2. Prefer final subclasses when possible
3. Use non-sealed only when necessary
4. Combine with pattern matching
5. Document the purpose of sealing
6. Keep permitted classes in same package when possible
7. Use sealed interfaces for extensible APIs


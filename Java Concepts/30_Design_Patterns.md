# Design Patterns

## What are Design Patterns?
Design patterns are reusable solutions to common problems in software design. They provide templates for solving design issues.

## Creational Patterns

### Singleton Pattern
Ensures a class has only one instance.

```java
class Singleton {
    private static Singleton instance;
    
    private Singleton() {
        // Private constructor
    }
    
    public static Singleton getInstance() {
        if (instance == null) {
            synchronized (Singleton.class) {
                if (instance == null) {
                    instance = new Singleton();
                }
            }
        }
        return instance;
    }
}

// Usage
Singleton singleton = Singleton.getInstance();
```

### Factory Pattern
Creates objects without specifying exact class.

```java
interface Shape {
    void draw();
}

class Circle implements Shape {
    @Override
    public void draw() {
        System.out.println("Drawing Circle");
    }
}

class Rectangle implements Shape {
    @Override
    public void draw() {
        System.out.println("Drawing Rectangle");
    }
}

class ShapeFactory {
    public Shape createShape(String type) {
        if (type.equalsIgnoreCase("circle")) {
            return new Circle();
        } else if (type.equalsIgnoreCase("rectangle")) {
            return new Rectangle();
        }
        return null;
    }
}

// Usage
ShapeFactory factory = new ShapeFactory();
Shape shape = factory.createShape("circle");
shape.draw();
```

### Builder Pattern
Constructs complex objects step by step.

```java
class Pizza {
    private String dough;
    private String sauce;
    private String topping;
    
    private Pizza(Builder builder) {
        this.dough = builder.dough;
        this.sauce = builder.sauce;
        this.topping = builder.topping;
    }
    
    static class Builder {
        private String dough;
        private String sauce;
        private String topping;
        
        public Builder dough(String dough) {
            this.dough = dough;
            return this;
        }
        
        public Builder sauce(String sauce) {
            this.sauce = sauce;
            return this;
        }
        
        public Builder topping(String topping) {
            this.topping = topping;
            return this;
        }
        
        public Pizza build() {
            return new Pizza(this);
        }
    }
}

// Usage
Pizza pizza = new Pizza.Builder()
    .dough("thin")
    .sauce("tomato")
    .topping("cheese")
    .build();
```

## Structural Patterns

### Adapter Pattern
Allows incompatible interfaces to work together.

```java
interface MediaPlayer {
    void play(String audioType, String fileName);
}

interface AdvancedMediaPlayer {
    void playVlc(String fileName);
    void playMp4(String fileName);
}

class VlcPlayer implements AdvancedMediaPlayer {
    @Override
    public void playVlc(String fileName) {
        System.out.println("Playing vlc file: " + fileName);
    }
    
    @Override
    public void playMp4(String fileName) {
        // Do nothing
    }
}

class MediaAdapter implements MediaPlayer {
    AdvancedMediaPlayer advancedPlayer;
    
    public MediaAdapter(String audioType) {
        if (audioType.equalsIgnoreCase("vlc")) {
            advancedPlayer = new VlcPlayer();
        }
    }
    
    @Override
    public void play(String audioType, String fileName) {
        if (audioType.equalsIgnoreCase("vlc")) {
            advancedPlayer.playVlc(fileName);
        }
    }
}
```

### Decorator Pattern
Adds behavior to objects dynamically.

```java
interface Coffee {
    double cost();
    String description();
}

class SimpleCoffee implements Coffee {
    @Override
    public double cost() {
        return 2.0;
    }
    
    @Override
    public String description() {
        return "Simple Coffee";
    }
}

class MilkDecorator implements Coffee {
    private Coffee coffee;
    
    public MilkDecorator(Coffee coffee) {
        this.coffee = coffee;
    }
    
    @Override
    public double cost() {
        return coffee.cost() + 0.5;
    }
    
    @Override
    public String description() {
        return coffee.description() + ", Milk";
    }
}

// Usage
Coffee coffee = new MilkDecorator(new SimpleCoffee());
System.out.println(coffee.description() + ": $" + coffee.cost());
```

## Behavioral Patterns

### Observer Pattern
Notifies multiple objects about state changes.

```java
import java.util.*;

interface Observer {
    void update(String message);
}

interface Subject {
    void attach(Observer observer);
    void detach(Observer observer);
    void notifyObservers();
}

class NewsAgency implements Subject {
    private List<Observer> observers = new ArrayList<>();
    private String news;
    
    @Override
    public void attach(Observer observer) {
        observers.add(observer);
    }
    
    @Override
    public void detach(Observer observer) {
        observers.remove(observer);
    }
    
    @Override
    public void notifyObservers() {
        for (Observer observer : observers) {
            observer.update(news);
        }
    }
    
    public void setNews(String news) {
        this.news = news;
        notifyObservers();
    }
}

class NewsChannel implements Observer {
    private String name;
    
    public NewsChannel(String name) {
        this.name = name;
    }
    
    @Override
    public void update(String message) {
        System.out.println(name + " received: " + message);
    }
}

// Usage
NewsAgency agency = new NewsAgency();
agency.attach(new NewsChannel("CNN"));
agency.attach(new NewsChannel("BBC"));
agency.setNews("Breaking news!");
```

### Strategy Pattern
Defines a family of algorithms and makes them interchangeable.

```java
interface PaymentStrategy {
    void pay(double amount);
}

class CreditCardPayment implements PaymentStrategy {
    private String cardNumber;
    
    public CreditCardPayment(String cardNumber) {
        this.cardNumber = cardNumber;
    }
    
    @Override
    public void pay(double amount) {
        System.out.println("Paid $" + amount + " using Credit Card");
    }
}

class PayPalPayment implements PaymentStrategy {
    private String email;
    
    public PayPalPayment(String email) {
        this.email = email;
    }
    
    @Override
    public void pay(double amount) {
        System.out.println("Paid $" + amount + " using PayPal");
    }
}

class ShoppingCart {
    private PaymentStrategy paymentStrategy;
    
    public void setPaymentStrategy(PaymentStrategy strategy) {
        this.paymentStrategy = strategy;
    }
    
    public void checkout(double amount) {
        paymentStrategy.pay(amount);
    }
}

// Usage
ShoppingCart cart = new ShoppingCart();
cart.setPaymentStrategy(new CreditCardPayment("1234-5678"));
cart.checkout(100.0);
```

## Best Practices
1. Understand the problem before applying a pattern
2. Don't overuse patterns - keep code simple
3. Patterns should solve real problems, not be forced
4. Consider trade-offs (complexity vs flexibility)
5. Study common patterns used in your framework
6. Refactor existing code to patterns when it makes sense
7. Document why a pattern is used


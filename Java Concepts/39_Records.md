# Records (Java 14+)

## What are Records?
Records are immutable data classes that automatically generate boilerplate code (constructors, getters, equals, hashCode, toString).

## Basic Record
```java
record Point(int x, int y) {
}

// Usage
Point p1 = new Point(10, 20);
Point p2 = new Point(10, 20);

System.out.println(p1.x()); // 10
System.out.println(p1.y()); // 20
System.out.println(p1.equals(p2)); // true
System.out.println(p1); // Point[x=10, y=20]
```

## Record with Methods
```java
record Person(String name, int age) {
    // Compact constructor (validation)
    public Person {
        if (age < 0) {
            throw new IllegalArgumentException("Age cannot be negative");
        }
    }
    
    // Instance method
    public boolean isAdult() {
        return age >= 18;
    }
    
    // Static method
    public static Person create(String name, int age) {
        return new Person(name, age);
    }
}

// Usage
Person person = new Person("John", 25);
System.out.println(person.isAdult()); // true
```

## Record with Custom Implementations
```java
record Student(String name, int id) {
    // Custom toString
    @Override
    public String toString() {
        return String.format("Student[name=%s, id=%d]", name, id);
    }
    
    // Custom equals (rarely needed)
    @Override
    public boolean equals(Object obj) {
        if (this == obj) return true;
        if (obj == null || getClass() != obj.getClass()) return false;
        Student student = (Student) obj;
        return id == student.id && name.equals(student.name);
    }
}
```

## Record with Nested Records
```java
record Address(String street, String city, String zip) {}

record Person(String name, int age, Address address) {}

// Usage
Address address = new Address("123 Main St", "NYC", "10001");
Person person = new Person("John", 25, address);
System.out.println(person.address().city()); // "NYC"
```

## Record in Collections
```java
record Product(String name, double price) {}

List<Product> products = Arrays.asList(
    new Product("Laptop", 999.99),
    new Product("Mouse", 29.99),
    new Product("Keyboard", 79.99)
);

// Filter
List<Product> expensive = products.stream()
    .filter(p -> p.price() > 100)
    .toList();

// Map
List<String> names = products.stream()
    .map(Product::name)
    .toList();
```

## Record with Generics
```java
record Pair<T, U>(T first, U second) {
    public Pair<U, T> swap() {
        return new Pair<>(second, first);
    }
}

// Usage
Pair<String, Integer> pair = new Pair<>("Hello", 42);
Pair<Integer, String> swapped = pair.swap();
```

## Record Implementing Interface
```java
interface Drawable {
    void draw();
}

record Circle(int radius) implements Drawable {
    @Override
    public void draw() {
        System.out.println("Drawing circle with radius: " + radius);
    }
}

// Usage
Circle circle = new Circle(5);
circle.draw();
```

## Record with Validation
```java
record Email(String value) {
    public Email {
        if (value == null || !value.contains("@")) {
            throw new IllegalArgumentException("Invalid email");
        }
    }
}

// Usage
Email email = new Email("user@example.com");
```

## Record in Switch Expressions (Java 17+)
```java
record Point(int x, int y) {}

Point point = new Point(5, 10);

String quadrant = switch (point) {
    case Point(int x, int y) when x > 0 && y > 0 -> "First";
    case Point(int x, int y) when x < 0 && y > 0 -> "Second";
    case Point(int x, int y) when x < 0 && y < 0 -> "Third";
    case Point(int x, int y) when x > 0 && y < 0 -> "Fourth";
    default -> "Origin";
};
```

## Record vs Class
```java
// Old way (Class)
class Point {
    private final int x;
    private final int y;
    
    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }
    
    public int getX() { return x; }
    public int getY() { return y; }
    
    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        Point point = (Point) o;
        return x == point.x && y == point.y;
    }
    
    @Override
    public int hashCode() {
        return Objects.hash(x, y);
    }
    
    @Override
    public String toString() {
        return "Point{x=" + x + ", y=" + y + "}";
    }
}

// New way (Record)
record Point(int x, int y) {}
```

## Best Practices
1. Use records for immutable data carriers
2. Don't use records for mutable classes
3. Add validation in compact constructor
4. Use records with pattern matching
5. Prefer records over classes for DTOs
6. Keep records simple and focused
7. Use records in functional programming
8. Consider records for value objects


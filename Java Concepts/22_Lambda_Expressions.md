# Lambda Expressions

## What are Lambda Expressions?
Lambda expressions provide a concise way to represent anonymous functions (methods without a name). They enable functional programming in Java.

## Syntax
```java
(parameters) -> expression
(parameters) -> { statements; }
```

## Basic Examples

### Before Lambda (Anonymous Inner Class)
```java
Runnable r = new Runnable() {
    @Override
    public void run() {
        System.out.println("Hello");
    }
};
```

### With Lambda
```java
Runnable r = () -> System.out.println("Hello");
```

## Functional Interfaces
Lambda expressions work with functional interfaces (interfaces with exactly one abstract method).

### Built-in Functional Interfaces

#### Predicate<T>
Tests a condition, returns boolean.

```java
import java.util.function.Predicate;

Predicate<Integer> isEven = n -> n % 2 == 0;
System.out.println(isEven.test(4)); // true
System.out.println(isEven.test(5)); // false
```

#### Function<T, R>
Takes one argument, returns a result.

```java
import java.util.function.Function;

Function<String, Integer> length = s -> s.length();
System.out.println(length.apply("Hello")); // 5

Function<Integer, Integer> square = x -> x * x;
System.out.println(square.apply(5)); // 25
```

#### Consumer<T>
Takes one argument, returns nothing.

```java
import java.util.function.Consumer;

Consumer<String> printer = s -> System.out.println(s);
printer.accept("Hello World");

Consumer<Integer> doubleIt = n -> System.out.println(n * 2);
doubleIt.accept(5); // 10
```

#### Supplier<T>
Takes no arguments, returns a value.

```java
import java.util.function.Supplier;

Supplier<String> greeting = () -> "Hello";
System.out.println(greeting.get());

Supplier<Double> random = () -> Math.random();
System.out.println(random.get());
```

#### BiFunction<T, U, R>
Takes two arguments, returns a result.

```java
import java.util.function.BiFunction;

BiFunction<Integer, Integer, Integer> add = (a, b) -> a + b;
System.out.println(add.apply(5, 3)); // 8

BiFunction<String, String, String> concat = (a, b) -> a + b;
System.out.println(concat.apply("Hello", " World")); // "Hello World"
```

## Lambda with Collections

### List Operations
```java
import java.util.*;

List<String> names = Arrays.asList("Alice", "Bob", "Charlie");

// ForEach
names.forEach(name -> System.out.println(name));
names.forEach(System.out::println); // Method reference

// Filter
List<String> filtered = new ArrayList<>();
names.forEach(name -> {
    if (name.length() > 4) {
        filtered.add(name);
    }
});
```

### Comparator
```java
List<String> names = Arrays.asList("Charlie", "Alice", "Bob");

// Sort by length
Collections.sort(names, (a, b) -> a.length() - b.length());

// Sort alphabetically
Collections.sort(names, (a, b) -> a.compareTo(b));
```

## Method References

### Static Method Reference
```java
Function<String, Integer> parser = Integer::parseInt;
int num = parser.apply("123");
```

### Instance Method Reference
```java
String str = "Hello";
Function<Integer, Character> charAt = str::charAt;
char c = charAt.apply(0); // 'H'
```

### Constructor Reference
```java
Supplier<List<String>> listSupplier = ArrayList::new;
List<String> list = listSupplier.get();
```

## Lambda Examples

### Event Handling
```java
button.setOnAction(event -> {
    System.out.println("Button clicked");
    // Handle click
});
```

### Thread Creation
```java
Thread thread = new Thread(() -> {
    System.out.println("Thread running");
});
thread.start();
```

### Custom Functional Interface
```java
@FunctionalInterface
interface Calculator {
    int calculate(int a, int b);
}

Calculator add = (a, b) -> a + b;
Calculator multiply = (a, b) -> a * b;

System.out.println(add.calculate(5, 3)); // 8
System.out.println(multiply.calculate(5, 3)); // 15
```

### Complex Lambda
```java
Function<String, String> process = s -> {
    String trimmed = s.trim();
    String upper = trimmed.toUpperCase();
    return upper;
};

String result = process.apply("  hello world  "); // "HELLO WORLD"
```

## Variable Capture
Lambda expressions can access:
- Local variables (must be final or effectively final)
- Instance variables
- Static variables

```java
int multiplier = 10; // Effectively final
Function<Integer, Integer> multiply = x -> x * multiplier;
// multiplier = 20; // Error: would break effectively final
```

## Chaining Functions
```java
Function<String, String> trim = String::trim;
Function<String, String> upper = String::toUpperCase;
Function<String, String> addPrefix = s -> "PREFIX: " + s;

Function<String, String> pipeline = trim
    .andThen(upper)
    .andThen(addPrefix);

String result = pipeline.apply("  hello  "); // "PREFIX: HELLO"
```

## Complete Example
```java
import java.util.*;
import java.util.function.*;

public class LambdaDemo {
    public static void main(String[] args) {
        List<Person> people = Arrays.asList(
            new Person("Alice", 25),
            new Person("Bob", 30),
            new Person("Charlie", 20)
        );
        
        // Sort by age
        people.sort((p1, p2) -> p1.getAge() - p2.getAge());
        
        // Filter adults
        Predicate<Person> isAdult = p -> p.getAge() >= 18;
        people.stream()
              .filter(isAdult)
              .forEach(p -> System.out.println(p.getName()));
        
        // Transform names to uppercase
        Function<Person, String> getName = Person::getName;
        Function<String, String> toUpper = String::toUpperCase;
        
        people.stream()
              .map(getName.andThen(toUpper))
              .forEach(System.out::println);
    }
}

class Person {
    private String name;
    private int age;
    
    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }
    
    public String getName() { return name; }
    public int getAge() { return age; }
}
```

## Best Practices
1. Keep lambda expressions short and readable
2. Use method references when appropriate
3. Extract complex lambdas into methods
4. Use meaningful parameter names
5. Prefer functional interfaces from java.util.function
6. Avoid side effects in lambda expressions
7. Use @FunctionalInterface annotation for custom interfaces


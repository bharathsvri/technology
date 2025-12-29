# Var Keyword (Java 10+)

## What is Var?
The `var` keyword enables local variable type inference. The compiler infers the type from the initializer.

## Basic Usage
```java
// Instead of explicit type
String message = "Hello";
List<String> names = new ArrayList<>();

// Using var
var message = "Hello";           // Inferred as String
var names = new ArrayList<>();  // Inferred as ArrayList<Object>
var numbers = new ArrayList<Integer>(); // Inferred as ArrayList<Integer>
```

## Valid Uses

### Local Variables
```java
public void method() {
    var name = "John";           // String
    var age = 25;                // int
    var price = 19.99;           // double
    var isActive = true;         // boolean
    var list = new ArrayList<String>(); // ArrayList<String>
}
```

### Enhanced For Loop
```java
var numbers = Arrays.asList(1, 2, 3, 4, 5);

for (var num : numbers) {
    System.out.println(num);
}
```

### Traditional For Loop
```java
for (var i = 0; i < 10; i++) {
    System.out.println(i);
}
```

### Try-With-Resources
```java
try (var reader = new BufferedReader(new FileReader("file.txt"))) {
    String line = reader.readLine();
}
```

## Invalid Uses

### Cannot Use Without Initializer
```java
var name; // Error: cannot infer type
var age = null; // Error: cannot infer type from null
```

### Cannot Use for Method Parameters
```java
public void method(var param) { // Error: not allowed
}
```

### Cannot Use for Return Types
```java
public var getValue() { // Error: not allowed
    return "value";
}
```

### Cannot Use for Fields
```java
class MyClass {
    var field; // Error: not allowed
}
```

### Cannot Use for Lambda Parameters (Java 10)
```java
// Java 10 - Error
var lambda = (var x, var y) -> x + y;

// Java 11+ - Allowed
var lambda = (var x, var y) -> x + y;
```

## When to Use Var

### Good Use Cases
```java
// Complex generic types
var map = new HashMap<String, List<Person>>();

// Builder pattern
var person = Person.builder()
    .name("John")
    .age(25)
    .build();

// Long class names
var connection = DriverManager.getConnection(url);
```

### Avoid When
```java
// Type is not obvious
var data = getData(); // What type is data?

// Better with explicit type
List<Person> data = getData(); // Clear what type
```

## Best Practices
1. Use var when type is obvious from context
2. Don't use var when type is unclear
3. Use meaningful variable names
4. Prefer explicit types for public APIs
5. Use var for complex generic types
6. Don't use var just to save typing
7. Consider readability over brevity

## Examples

### With Collections
```java
var list = new ArrayList<String>();
var set = new HashSet<Integer>();
var map = new HashMap<String, Integer>();
```

### With Streams
```java
var numbers = Arrays.asList(1, 2, 3, 4, 5);
var result = numbers.stream()
    .filter(n -> n % 2 == 0)
    .map(n -> n * 2)
    .collect(Collectors.toList());
```

### With Optional
```java
var optional = Optional.of("Hello");
var value = optional.orElse("Default");
```

## Complete Example
```java
import java.util.*;

public class VarDemo {
    public static void main(String[] args) {
        // Local variables
        var name = "John";
        var age = 25;
        var isActive = true;
        
        // Collections
        var list = new ArrayList<String>();
        list.add("Apple");
        list.add("Banana");
        
        // Enhanced for loop
        for (var item : list) {
            System.out.println(item);
        }
        
        // Try-with-resources
        try (var scanner = new Scanner(System.in)) {
            var input = scanner.nextLine();
            System.out.println("Input: " + input);
        }
        
        // Complex types
        var map = new HashMap<String, List<Integer>>();
        map.put("numbers", Arrays.asList(1, 2, 3));
    }
}
```


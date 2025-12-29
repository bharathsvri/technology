# Optional Class

## What is Optional?
Optional (Java 8+) is a container object that may or may not contain a non-null value. It helps avoid NullPointerException and makes code more expressive.

## Creating Optional

### Empty Optional
```java
import java.util.Optional;

Optional<String> empty = Optional.empty();
```

### Optional with Value
```java
Optional<String> optional = Optional.of("Hello");
// Throws NullPointerException if value is null

Optional<String> nullable = Optional.ofNullable(null);
// Returns empty Optional if value is null
```

## Checking Value Presence
```java
Optional<String> optional = Optional.of("Hello");

boolean isPresent = optional.isPresent(); // true
boolean isEmpty = optional.isEmpty();     // false (Java 11+)
```

## Getting Value

### get()
```java
Optional<String> optional = Optional.of("Hello");
String value = optional.get(); // "Hello"

Optional<String> empty = Optional.empty();
String value2 = empty.get(); // Throws NoSuchElementException
```

### orElse()
```java
Optional<String> optional = Optional.of("Hello");
String value = optional.orElse("Default"); // "Hello"

Optional<String> empty = Optional.empty();
String value2 = empty.orElse("Default"); // "Default"
```

### orElseGet()
```java
Optional<String> empty = Optional.empty();
String value = empty.orElseGet(() -> "Computed Default");
// Lazy evaluation - supplier only called if empty
```

### orElseThrow()
```java
Optional<String> empty = Optional.empty();

// Default exception
String value = empty.orElseThrow(); // NoSuchElementException

// Custom exception
String value2 = empty.orElseThrow(() -> 
    new IllegalArgumentException("Value not found"));
```

## Conditional Operations

### ifPresent()
```java
Optional<String> optional = Optional.of("Hello");
optional.ifPresent(value -> System.out.println(value)); // Prints "Hello"

Optional<String> empty = Optional.empty();
empty.ifPresent(value -> System.out.println(value)); // Does nothing
```

### ifPresentOrElse() (Java 9+)
```java
Optional<String> optional = Optional.of("Hello");
optional.ifPresentOrElse(
    value -> System.out.println(value),
    () -> System.out.println("No value")
);

Optional<String> empty = Optional.empty();
empty.ifPresentOrElse(
    value -> System.out.println(value),
    () -> System.out.println("No value") // Executes this
);
```

## Transforming Values

### map()
```java
Optional<String> optional = Optional.of("hello");
Optional<String> upper = optional.map(String::toUpperCase);
System.out.println(upper.get()); // "HELLO"

Optional<String> empty = Optional.empty();
Optional<String> upper2 = empty.map(String::toUpperCase);
System.out.println(upper2.isPresent()); // false
```

### flatMap()
```java
Optional<String> optional = Optional.of("hello");
Optional<Integer> length = optional.flatMap(s -> 
    Optional.of(s.length()));
System.out.println(length.get()); // 5
```

### filter()
```java
Optional<String> optional = Optional.of("hello");
Optional<String> filtered = optional.filter(s -> s.length() > 3);
System.out.println(filtered.get()); // "hello"

Optional<String> filtered2 = optional.filter(s -> s.length() > 10);
System.out.println(filtered2.isPresent()); // false
```

## Chaining Operations
```java
Optional<String> optional = Optional.of("  hello  ");

String result = optional
    .map(String::trim)
    .map(String::toUpperCase)
    .filter(s -> s.length() > 3)
    .orElse("DEFAULT");

System.out.println(result); // "HELLO"
```

## Practical Examples

### Avoiding Null Checks
```java
// Old way
public String getValue(String key) {
    Map<String, String> map = getMap();
    String value = map.get(key);
    if (value != null) {
        return value.toUpperCase();
    }
    return "DEFAULT";
}

// With Optional
public String getValue(String key) {
    return Optional.ofNullable(getMap().get(key))
        .map(String::toUpperCase)
        .orElse("DEFAULT");
}
```

### Method Return Type
```java
public Optional<User> findUser(String id) {
    // Database lookup
    User user = database.find(id);
    return Optional.ofNullable(user);
}

// Usage
Optional<User> user = findUser("123");
user.ifPresent(u -> System.out.println(u.getName()));
```

### Chaining Optional Calls
```java
class Address {
    private String street;
    public Optional<String> getStreet() {
        return Optional.ofNullable(street);
    }
}

class Person {
    private Address address;
    public Optional<Address> getAddress() {
        return Optional.ofNullable(address);
    }
}

// Safe navigation
Optional<Person> person = findPerson("123");
Optional<String> street = person
    .flatMap(Person::getAddress)
    .flatMap(Address::getStreet);
```

## Complete Example
```java
import java.util.Optional;

class User {
    private String name;
    private String email;
    
    public User(String name, String email) {
        this.name = name;
        this.email = email;
    }
    
    public Optional<String> getName() {
        return Optional.ofNullable(name);
    }
    
    public Optional<String> getEmail() {
        return Optional.ofNullable(email);
    }
}

public class OptionalDemo {
    public static void main(String[] args) {
        // Create Optional
        Optional<User> user = Optional.of(new User("John", "john@example.com"));
        
        // Get name safely
        String name = user
            .flatMap(User::getName)
            .orElse("Unknown");
        System.out.println("Name: " + name);
        
        // Get email in uppercase
        user.flatMap(User::getEmail)
            .map(String::toUpperCase)
            .ifPresent(email -> System.out.println("Email: " + email));
        
        // Filter
        Optional<User> validUser = user.filter(u -> 
            u.getEmail().isPresent());
        
        // Empty Optional
        Optional<User> empty = Optional.empty();
        String defaultName = empty
            .flatMap(User::getName)
            .orElse("Guest");
        System.out.println("Default: " + defaultName);
    }
}
```

## Best Practices
1. **Don't use Optional for fields** - use it for return types
2. **Don't use Optional.get()** without checking - use orElse() or similar
3. **Don't use Optional for collections** - use empty collections instead
4. **Use Optional for return types** to indicate possible absence
5. **Prefer orElseGet() over orElse()** when computation is expensive
6. **Use map() and flatMap()** for transformations
7. **Avoid nesting Optional** - use flatMap() instead
8. **Don't use Optional as method parameter** - use overloading instead

## Common Patterns

### Null-Safe Chain
```java
Optional.ofNullable(obj)
    .map(Object::getValue)
    .map(String::toUpperCase)
    .orElse("DEFAULT");
```

### Validation
```java
Optional<String> valid = Optional.ofNullable(input)
    .filter(s -> !s.isEmpty())
    .filter(s -> s.length() > 3);
```

### Default Value with Computation
```java
String value = optional.orElseGet(() -> 
    expensiveComputation());
```


# Streams API

## What are Streams?
Streams API (Java 8+) provides a functional approach to process collections of objects. It allows declarative processing of data in a functional style.

## Stream Characteristics
- Not a data structure
- Functional in nature (doesn't modify source)
- Lazy evaluation
- Can be consumed only once
- Parallelizable

## Creating Streams

### From Collections
```java
List<String> list = Arrays.asList("a", "b", "c");
Stream<String> stream = list.stream();
```

### From Arrays
```java
String[] array = {"a", "b", "c"};
Stream<String> stream = Arrays.stream(array);
```

### Using Stream.of()
```java
Stream<String> stream = Stream.of("a", "b", "c");
```

### Using Stream.builder()
```java
Stream<String> stream = Stream.<String>builder()
    .add("a")
    .add("b")
    .add("c")
    .build();
```

### Infinite Streams
```java
Stream<Integer> infinite = Stream.iterate(0, n -> n + 2);
Stream<Double> random = Stream.generate(Math::random);
```

## Intermediate Operations
Return a new stream, can be chained.

### Filter
```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6);
List<Integer> even = numbers.stream()
    .filter(n -> n % 2 == 0)
    .collect(Collectors.toList());
```

### Map
```java
List<String> names = Arrays.asList("alice", "bob", "charlie");
List<String> upper = names.stream()
    .map(String::toUpperCase)
    .collect(Collectors.toList());
```

### FlatMap
```java
List<List<String>> lists = Arrays.asList(
    Arrays.asList("a", "b"),
    Arrays.asList("c", "d")
);
List<String> flat = lists.stream()
    .flatMap(List::stream)
    .collect(Collectors.toList());
```

### Distinct
```java
List<Integer> numbers = Arrays.asList(1, 2, 2, 3, 3, 4);
List<Integer> unique = numbers.stream()
    .distinct()
    .collect(Collectors.toList());
```

### Sorted
```java
List<String> names = Arrays.asList("Charlie", "Alice", "Bob");
List<String> sorted = names.stream()
    .sorted()
    .collect(Collectors.toList());

// Custom comparator
List<String> sortedByLength = names.stream()
    .sorted((a, b) -> a.length() - b.length())
    .collect(Collectors.toList());
```

### Limit and Skip
```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8);
List<Integer> first3 = numbers.stream()
    .limit(3)
    .collect(Collectors.toList());

List<Integer> skip3 = numbers.stream()
    .skip(3)
    .collect(Collectors.toList());
```

### Peek
```java
List<String> names = Arrays.asList("Alice", "Bob");
List<String> result = names.stream()
    .peek(System.out::println)
    .map(String::toUpperCase)
    .collect(Collectors.toList());
```

## Terminal Operations
Produce a result or side effect, terminate the stream.

### ForEach
```java
List<String> names = Arrays.asList("Alice", "Bob", "Charlie");
names.stream().forEach(System.out::println);
```

### Collect
```java
List<String> names = Arrays.asList("Alice", "Bob", "Charlie");
List<String> result = names.stream()
    .map(String::toUpperCase)
    .collect(Collectors.toList());

// To Set
Set<String> set = names.stream()
    .collect(Collectors.toSet());

// To Map
Map<String, Integer> map = names.stream()
    .collect(Collectors.toMap(
        name -> name,
        String::length
    ));
```

### Reduce
```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);
Optional<Integer> sum = numbers.stream()
    .reduce((a, b) -> a + b);
// Or
int sum2 = numbers.stream()
    .reduce(0, Integer::sum);
```

### Count
```java
List<String> names = Arrays.asList("Alice", "Bob", "Charlie");
long count = names.stream().count();
```

### AnyMatch, AllMatch, NoneMatch
```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);
boolean hasEven = numbers.stream().anyMatch(n -> n % 2 == 0);
boolean allPositive = numbers.stream().allMatch(n -> n > 0);
boolean noneNegative = numbers.stream().noneMatch(n -> n < 0);
```

### FindFirst, FindAny
```java
List<String> names = Arrays.asList("Alice", "Bob", "Charlie");
Optional<String> first = names.stream().findFirst();
Optional<String> any = names.stream().findAny();
```

### Min, Max
```java
List<Integer> numbers = Arrays.asList(1, 5, 3, 2, 4);
Optional<Integer> min = numbers.stream().min(Integer::compare);
Optional<Integer> max = numbers.stream().max(Integer::compare);
```

## Collectors

### Grouping
```java
List<Person> people = Arrays.asList(
    new Person("Alice", 25),
    new Person("Bob", 30),
    new Person("Charlie", 25)
);

Map<Integer, List<Person>> byAge = people.stream()
    .collect(Collectors.groupingBy(Person::getAge));
```

### Partitioning
```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6);
Map<Boolean, List<Integer>> partitioned = numbers.stream()
    .collect(Collectors.partitioningBy(n -> n % 2 == 0));
```

### Joining
```java
List<String> names = Arrays.asList("Alice", "Bob", "Charlie");
String joined = names.stream()
    .collect(Collectors.joining(", "));
// "Alice, Bob, Charlie"
```

### Summarizing
```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);
IntSummaryStatistics stats = numbers.stream()
    .collect(Collectors.summarizingInt(Integer::intValue));
// stats.getSum(), stats.getAverage(), stats.getMax(), etc.
```

## Parallel Streams
```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8);
List<Integer> doubled = numbers.parallelStream()
    .map(n -> n * 2)
    .collect(Collectors.toList());
```

## Complete Example
```java
import java.util.*;
import java.util.stream.*;

class Person {
    private String name;
    private int age;
    private String city;
    
    public Person(String name, int age, String city) {
        this.name = name;
        this.age = age;
        this.city = city;
    }
    
    public String getName() { return name; }
    public int getAge() { return age; }
    public String getCity() { return city; }
}

public class StreamsDemo {
    public static void main(String[] args) {
        List<Person> people = Arrays.asList(
            new Person("Alice", 25, "NYC"),
            new Person("Bob", 30, "LA"),
            new Person("Charlie", 25, "NYC"),
            new Person("David", 35, "LA")
        );
        
        // Filter adults
        List<Person> adults = people.stream()
            .filter(p -> p.getAge() >= 18)
            .collect(Collectors.toList());
        
        // Get names in uppercase
        List<String> names = people.stream()
            .map(Person::getName)
            .map(String::toUpperCase)
            .collect(Collectors.toList());
        
        // Group by city
        Map<String, List<Person>> byCity = people.stream()
            .collect(Collectors.groupingBy(Person::getCity));
        
        // Average age
        double avgAge = people.stream()
            .mapToInt(Person::getAge)
            .average()
            .orElse(0.0);
        
        // Oldest person
        Optional<Person> oldest = people.stream()
            .max(Comparator.comparing(Person::getAge));
    }
}
```

## Best Practices
1. Use streams for declarative data processing
2. Keep intermediate operations stateless
3. Prefer method references over lambdas when possible
4. Use parallel streams only for large datasets
5. Avoid side effects in stream operations
6. Use appropriate collectors for terminal operations
7. Chain operations for readability


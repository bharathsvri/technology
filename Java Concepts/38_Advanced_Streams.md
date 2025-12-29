# Advanced Streams

## Custom Collectors
```java
import java.util.stream.Collector;
import java.util.stream.Collectors;
import java.util.*;

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

// Custom collector
Collector<Person, ?, Map<String, List<Person>>> byName = 
    Collector.of(
        HashMap::new,
        (map, person) -> {
            String key = person.getName().substring(0, 1);
            map.computeIfAbsent(key, k -> new ArrayList<>()).add(person);
        },
        (map1, map2) -> {
            map2.forEach((key, value) -> 
                map1.merge(key, value, (list1, list2) -> {
                    list1.addAll(list2);
                    return list1;
                }));
            return map1;
        }
    );

List<Person> people = Arrays.asList(
    new Person("Alice", 25),
    new Person("Bob", 30),
    new Person("Charlie", 20)
);

Map<String, List<Person>> grouped = people.stream().collect(byName);
```

## Parallel Streams
```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);

// Parallel processing
int sum = numbers.parallelStream()
    .mapToInt(Integer::intValue)
    .sum();

// With custom thread pool
ForkJoinPool customPool = new ForkJoinPool(4);
customPool.submit(() -> {
    int result = numbers.parallelStream()
        .mapToInt(Integer::intValue)
        .sum();
    return result;
});
```

## Stream Reduction
```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);

// Custom reduction
Integer sum = numbers.stream()
    .reduce(0, (a, b) -> a + b);

// With identity and accumulator
Integer product = numbers.stream()
    .reduce(1, (a, b) -> a * b);

// With combiner (for parallel streams)
Integer sum2 = numbers.parallelStream()
    .reduce(0, (a, b) -> a + b, (a, b) -> a + b);
```

## Grouping and Partitioning
```java
List<Person> people = Arrays.asList(
    new Person("Alice", 25, "NYC"),
    new Person("Bob", 30, "LA"),
    new Person("Charlie", 25, "NYC")
);

// Grouping by
Map<Integer, List<Person>> byAge = people.stream()
    .collect(Collectors.groupingBy(Person::getAge));

// Grouping by with downstream collector
Map<Integer, Long> countByAge = people.stream()
    .collect(Collectors.groupingBy(
        Person::getAge, 
        Collectors.counting()));

// Grouping by with mapping
Map<Integer, List<String>> namesByAge = people.stream()
    .collect(Collectors.groupingBy(
        Person::getAge,
        Collectors.mapping(Person::getName, Collectors.toList())));

// Partitioning
Map<Boolean, List<Person>> partitioned = people.stream()
    .collect(Collectors.partitioningBy(p -> p.getAge() >= 25));
```

## Collecting And Then
```java
List<String> names = Arrays.asList("Alice", "Bob", "Charlie");

// Apply function after collection
String result = names.stream()
    .collect(Collectors.collectingAndThen(
        Collectors.joining(", "),
        s -> "[" + s + "]"
    ));
// Result: "[Alice, Bob, Charlie]"
```

## FlatMap Advanced
```java
List<List<Integer>> lists = Arrays.asList(
    Arrays.asList(1, 2, 3),
    Arrays.asList(4, 5, 6),
    Arrays.asList(7, 8, 9)
);

// Flatten and process
List<Integer> flattened = lists.stream()
    .flatMap(List::stream)
    .collect(Collectors.toList());

// FlatMap with Optional
List<String> words = Arrays.asList("hello", "world", "java");
List<Character> chars = words.stream()
    .flatMap(word -> word.chars().mapToObj(c -> (char) c))
    .collect(Collectors.toList());
```

## Stream Teeing (Java 12+)
```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);

Map<String, Object> stats = numbers.stream()
    .collect(Collectors.teeing(
        Collectors.summingInt(Integer::intValue),
        Collectors.counting(),
        (sum, count) -> Map.of(
            "sum", sum,
            "count", count,
            "average", sum / count.doubleValue()
        )
    ));
```

## Infinite Streams
```java
// Generate infinite stream
Stream<Integer> infinite = Stream.iterate(0, n -> n + 2)
    .limit(10); // Limit to 10 elements

// Fibonacci sequence
Stream.iterate(new int[]{0, 1}, t -> new int[]{t[1], t[0] + t[1]})
    .limit(10)
    .map(t -> t[0])
    .forEach(System.out::println);

// Random numbers
Stream.generate(() -> Math.random())
    .limit(10)
    .forEach(System.out::println);
```

## Stream Concatenation
```java
Stream<String> stream1 = Stream.of("A", "B", "C");
Stream<String> stream2 = Stream.of("D", "E", "F");

Stream<String> combined = Stream.concat(stream1, stream2);
```

## Primitive Streams
```java
// IntStream
IntStream.range(1, 10)
    .filter(n -> n % 2 == 0)
    .forEach(System.out::println);

// LongStream
long sum = LongStream.range(1, 100)
    .sum();

// DoubleStream
double average = DoubleStream.of(1.0, 2.0, 3.0, 4.0)
    .average()
    .orElse(0.0);
```

## Best Practices
1. Use parallel streams for CPU-intensive tasks
2. Avoid side effects in stream operations
3. Use primitive streams for better performance
4. Prefer method references over lambdas
5. Use appropriate collectors
6. Be careful with infinite streams
7. Consider performance implications
8. Use teeing for multiple aggregations


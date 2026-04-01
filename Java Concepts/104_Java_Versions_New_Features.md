# Java Versions and New Features

## Why This Document?
This file gives a version-wise summary of important Java features so you can quickly revise what changed from Java 8 to Java 21.

## Java 8 (LTS) - Functional Programming Foundation
- Lambda expressions
- Functional interfaces
- Stream API
- Method references
- Optional
- New Date and Time API (`java.time`)
- Default and static methods in interfaces

```java
List<String> names = List.of("ram", "sita", "hari");
names.stream()
     .map(String::toUpperCase)
     .forEach(System.out::println);
```

## Java 9 - Modules and Platform Improvements
- Java Platform Module System (JPMS)
- JShell (interactive REPL)
- Improved Stream API (`takeWhile`, `dropWhile`, `iterate` with predicate)
- Factory methods (`List.of`, `Set.of`, `Map.of`)
- `Optional` enhancements

## Java 10
- `var` for local variable type inference

```java
var count = 10;          // int
var message = "Hello";   // String
```

## Java 11 (LTS)
- New HTTP Client API (`java.net.http`)
- `String` methods: `isBlank`, `lines`, `repeat`, `strip`
- `var` in lambda parameters
- Single-file source execution (`java MyFile.java`)

## Java 12 and 13
- Switch expressions (preview in 12, refined in 13)
- Text blocks (preview in 13)

## Java 14
- Records (preview)
- Switch expressions (standard)
- Helpful `NullPointerException` messages

## Java 15
- Text blocks (standard)
- Sealed classes (preview)

## Java 16
- Records (standard)
- Pattern matching for `instanceof` (standard)

```java
Object obj = "Java";
if (obj instanceof String s) {
    System.out.println(s.toUpperCase());
}
```

## Java 17 (LTS)
- Sealed classes (standard)
- Pattern matching improvements
- Strong encapsulation of JDK internals

```java
public sealed class Shape permits Circle, Rectangle {}
final class Circle extends Shape {}
final class Rectangle extends Shape {}
```

## Java 18 to 20 (Important Previews and Runtime Work)
- UTF-8 by default
- Structured concurrency (preview)
- Scoped values (preview)
- Pattern matching and switch improvements
- Virtual threads matured through previews

## Java 21 (LTS) - Modern Concurrency Milestone
- Virtual threads (standard)
- Pattern matching for switch (standard)
- Record patterns (standard)
- Sequenced collections
- String templates (preview)
- Unnamed patterns and variables (preview)

```java
try (var executor = Executors.newVirtualThreadPerTaskExecutor()) {
    IntStream.range(0, 5)
             .forEach(i -> executor.submit(() -> System.out.println("Task " + i)));
}
```

## LTS Versions to Focus
- Java 8
- Java 11
- Java 17
- Java 21

## Interview-Oriented Learning Order
1. Java 8: Lambda, Streams, Optional, `java.time`
2. Java 9-11: Modules, HTTP Client, `var`
3. Java 14-17: Records, Sealed Classes, Pattern Matching
4. Java 21: Virtual Threads and modern switch patterns

## Practical Advice
- Use Java 17 or Java 21 for new projects.
- Learn language features with small code examples.
- Understand "why" each feature exists (readability, safety, performance, concurrency).

# Pattern Matching (Java 17+)

## Instanceof Pattern Matching
```java
// Old way
if (obj instanceof String) {
    String str = (String) obj;
    System.out.println(str.length());
}

// New way (Java 16+)
if (obj instanceof String str) {
    System.out.println(str.length()); // No cast needed
}
```

## Pattern Matching in Switch (Java 17+)
```java
Object obj = "Hello";

String result = switch (obj) {
    case String s -> "String: " + s;
    case Integer i -> "Integer: " + i;
    case null -> "Null";
    default -> "Unknown";
};
```

## Type Patterns
```java
Number number = 42;

String type = switch (number) {
    case Integer i -> "Integer: " + i;
    case Double d -> "Double: " + d;
    case Long l -> "Long: " + l;
    default -> "Other number type";
};
```

## Guarded Patterns
```java
Object obj = 42;

String result = switch (obj) {
    case Integer i when i > 0 -> "Positive integer: " + i;
    case Integer i when i < 0 -> "Negative integer: " + i;
    case Integer i -> "Zero";
    case String s when s.length() > 5 -> "Long string: " + s;
    case String s -> "Short string: " + s;
    default -> "Other";
};
```

## Record Patterns (Java 19+)
```java
record Point(int x, int y) {}

Point point = new Point(5, 10);

// Deconstruct record
if (point instanceof Point(int x, int y)) {
    System.out.println("X: " + x + ", Y: " + y);
}

// In switch
String quadrant = switch (point) {
    case Point(int x, int y) when x > 0 && y > 0 -> "First quadrant";
    case Point(int x, int y) when x < 0 && y > 0 -> "Second quadrant";
    case Point(int x, int y) when x < 0 && y < 0 -> "Third quadrant";
    case Point(int x, int y) when x > 0 && y < 0 -> "Fourth quadrant";
    default -> "On axis";
};
```

## Nested Record Patterns
```java
record Point(int x, int y) {}
record Line(Point start, Point end) {}

Line line = new Line(new Point(0, 0), new Point(10, 10));

if (line instanceof Line(Point(int x1, int y1), Point(int x2, int y2))) {
    System.out.println("Line from (" + x1 + "," + y1 + ") to (" + x2 + "," + y2 + ")");
}
```

## Sealed Classes with Pattern Matching
```java
sealed class Expr permits ConstantExpr, AddExpr {
}

record ConstantExpr(int value) implements Expr {}
record AddExpr(Expr left, Expr right) implements Expr {}

int evaluate(Expr expr) {
    return switch (expr) {
        case ConstantExpr(int value) -> value;
        case AddExpr(Expr left, Expr right) -> 
            evaluate(left) + evaluate(right);
    };
}
```

## Pattern Matching with Collections
```java
List<Object> list = Arrays.asList("Hello", 42, 3.14);

for (Object item : list) {
    switch (item) {
        case String s -> System.out.println("String: " + s);
        case Integer i -> System.out.println("Integer: " + i);
        case Double d -> System.out.println("Double: " + d);
        default -> System.out.println("Other");
    }
}
```

## Exhaustive Pattern Matching
```java
sealed interface Result<T> permits Success, Failure {
}

record Success<T>(T value) implements Result<T> {}
record Failure<T>(String error) implements Result<T> {}

// Compiler ensures all cases are covered
String handle(Result<String> result) {
    return switch (result) {
        case Success<String>(String value) -> "Success: " + value;
        case Failure<String>(String error) -> "Failure: " + error;
    };
}
```

## Best Practices
1. Use pattern matching for cleaner code
2. Leverage exhaustive matching with sealed classes
3. Use guards for complex conditions
4. Prefer pattern matching over instanceof + cast
5. Use record patterns for data extraction
6. Combine with switch expressions
7. Let compiler check exhaustiveness


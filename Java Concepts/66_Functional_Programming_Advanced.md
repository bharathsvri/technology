# Functional Programming - Advanced

## Higher-Order Functions
Functions that take functions as parameters or return functions.

```java
import java.util.function.*;

// Function that takes a function
public static int applyOperation(int a, int b, BinaryOperator<Integer> op) {
    return op.apply(a, b);
}

// Usage
int sum = applyOperation(5, 3, (x, y) -> x + y);
int product = applyOperation(5, 3, (x, y) -> x * y);
```

## Function Composition
```java
import java.util.function.Function;

Function<Integer, Integer> addOne = x -> x + 1;
Function<Integer, Integer> multiplyByTwo = x -> x * 2;

// Compose functions
Function<Integer, Integer> addThenMultiply = addOne.andThen(multiplyByTwo);
// Result: (x + 1) * 2

Function<Integer, Integer> multiplyThenAdd = addOne.compose(multiplyByTwo);
// Result: (x * 2) + 1

int result = addThenMultiply.apply(5); // (5 + 1) * 2 = 12
```

## Currying
Converting a function with multiple arguments into a sequence of functions.

```java
import java.util.function.*;

// Normal function
BiFunction<Integer, Integer, Integer> add = (a, b) -> a + b;

// Curried function
Function<Integer, Function<Integer, Integer>> curriedAdd = 
    a -> b -> a + b;

// Usage
Function<Integer, Integer> addFive = curriedAdd.apply(5);
int result = addFive.apply(3); // 8
```

## Partial Application
```java
import java.util.function.*;

BiFunction<Integer, Integer, Integer> multiply = (a, b) -> a * b;

// Partially apply
Function<Integer, Integer> multiplyByTen = b -> multiply.apply(10, b);

int result = multiplyByTen.apply(5); // 50
```

## Monads

### Optional as Monad
```java
import java.util.Optional;

Optional<Integer> value = Optional.of(5);

Optional<Integer> result = value
    .map(x -> x * 2)
    .filter(x -> x > 5)
    .map(x -> x + 1);

result.ifPresent(System.out::println);
```

### Stream as Monad
```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);

List<Integer> result = numbers.stream()
    .map(x -> x * 2)
    .filter(x -> x > 5)
    .collect(Collectors.toList());
```

## Immutability
```java
// Immutable class
final class ImmutablePoint {
    private final int x;
    private final int y;
    
    public ImmutablePoint(int x, int y) {
        this.x = x;
        this.y = y;
    }
    
    public int getX() { return x; }
    public int getY() { return y; }
    
    // Return new instance instead of modifying
    public ImmutablePoint move(int dx, int dy) {
        return new ImmutablePoint(x + dx, y + dy);
    }
}
```

## Pure Functions
Functions that always return the same output for the same input and have no side effects.

```java
// Pure function
public static int add(int a, int b) {
    return a + b; // No side effects, deterministic
}

// Impure function
private static int counter = 0;
public static int increment() {
    return ++counter; // Side effect: modifies state
}
```

## Recursion
```java
// Tail recursion (simulated)
public static int factorial(int n) {
    return factorialTail(n, 1);
}

private static int factorialTail(int n, int acc) {
    if (n <= 1) return acc;
    return factorialTail(n - 1, n * acc);
}
```

## Best Practices
1. Prefer immutability
2. Use pure functions when possible
3. Compose functions for reusability
4. Use higher-order functions
5. Understand monads (Optional, Stream)
6. Use recursion appropriately
7. Avoid side effects
8. Write declarative code


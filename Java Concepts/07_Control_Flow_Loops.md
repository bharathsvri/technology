# Control Flow - Loops

## For Loop
Executes a block of code a fixed number of times.

### Syntax
```java
for (initialization; condition; increment/decrement) {
    // code block
}
```

### Example
```java
for (int i = 0; i < 5; i++) {
    System.out.println("Iteration: " + i);
}
// Output: 0, 1, 2, 3, 4
```

### Variations
```java
// Countdown
for (int i = 10; i > 0; i--) {
    System.out.println(i);
}

// Step by 2
for (int i = 0; i < 10; i += 2) {
    System.out.println(i);
}

// Multiple variables
for (int i = 0, j = 10; i < j; i++, j--) {
    System.out.println(i + " " + j);
}
```

## Enhanced For Loop (For-Each)
Iterates through arrays and collections.

### Syntax
```java
for (dataType variable : array/collection) {
    // code block
}
```

### Example
```java
int[] numbers = {1, 2, 3, 4, 5};
for (int num : numbers) {
    System.out.println(num);
}

String[] names = {"Alice", "Bob", "Charlie"};
for (String name : names) {
    System.out.println(name);
}
```

## While Loop
Executes code block while condition is true.

### Syntax
```java
while (condition) {
    // code block
}
```

### Example
```java
int i = 0;
while (i < 5) {
    System.out.println(i);
    i++;
}
```

### Infinite Loop
```java
while (true) {
    // code that runs forever
    // Use break to exit
}
```

## Do-While Loop
Executes code block at least once, then repeats while condition is true.

### Syntax
```java
do {
    // code block
} while (condition);
```

### Example
```java
int i = 0;
do {
    System.out.println(i);
    i++;
} while (i < 5);
```

## Loop Control Statements

### Break Statement
Exits the loop immediately.

```java
for (int i = 0; i < 10; i++) {
    if (i == 5) {
        break; // Exits loop when i == 5
    }
    System.out.println(i);
}
// Output: 0, 1, 2, 3, 4
```

### Continue Statement
Skips current iteration and continues with next.

```java
for (int i = 0; i < 10; i++) {
    if (i % 2 == 0) {
        continue; // Skips even numbers
    }
    System.out.println(i);
}
// Output: 1, 3, 5, 7, 9
```

### Labeled Break and Continue
Used with nested loops to break/continue outer loop.

```java
outer: for (int i = 0; i < 3; i++) {
    inner: for (int j = 0; j < 3; j++) {
        if (i == 1 && j == 1) {
            break outer; // Breaks outer loop
        }
        System.out.println(i + " " + j);
    }
}
```

## Nested Loops
Loop inside another loop.

```java
// Print multiplication table
for (int i = 1; i <= 5; i++) {
    for (int j = 1; j <= 5; j++) {
        System.out.print(i * j + "\t");
    }
    System.out.println();
}
```

## Common Loop Patterns

### Sum of Array Elements
```java
int[] numbers = {1, 2, 3, 4, 5};
int sum = 0;
for (int num : numbers) {
    sum += num;
}
System.out.println("Sum: " + sum);
```

### Finding Maximum
```java
int[] numbers = {3, 7, 2, 9, 1};
int max = numbers[0];
for (int num : numbers) {
    if (num > max) {
        max = num;
    }
}
System.out.println("Max: " + max);
```

### Searching
```java
int[] numbers = {1, 2, 3, 4, 5};
int target = 3;
boolean found = false;

for (int num : numbers) {
    if (num == target) {
        found = true;
        break;
    }
}
```

### Reverse Iteration
```java
int[] numbers = {1, 2, 3, 4, 5};
for (int i = numbers.length - 1; i >= 0; i--) {
    System.out.println(numbers[i]);
}
```

## Best Practices
1. **Use for-each** when iterating through collections
2. **Avoid infinite loops** unless intentional (with break condition)
3. **Initialize loop variables** properly
4. **Use meaningful variable names** (i, j, k are acceptable for simple loops)
5. **Consider performance** for large datasets
6. **Use break/continue** judiciously to improve readability

## Performance Tips
```java
// Good: Cache array length
int[] arr = new int[1000];
int len = arr.length;
for (int i = 0; i < len; i++) {
    // process
}

// Better: Use enhanced for loop
for (int num : arr) {
    // process
}
```


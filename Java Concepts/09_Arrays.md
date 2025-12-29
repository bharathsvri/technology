# Java Arrays

## What is an Array?
An array is a container object that holds a fixed number of values of a single type. Arrays are indexed starting from 0.

## Array Declaration

### One-Dimensional Array
```java
// Declaration
int[] numbers;
String[] names;

// Declaration and initialization
int[] numbers = {1, 2, 3, 4, 5};
String[] names = {"Alice", "Bob", "Charlie"};

// Declaration with size
int[] numbers = new int[5]; // Array of 5 integers (default: 0)
String[] names = new String[3]; // Array of 3 strings (default: null)
```

## Array Initialization
```java
// Initialize with values
int[] arr = {10, 20, 30, 40, 50};

// Initialize with new keyword
int[] arr = new int[]{10, 20, 30, 40, 50};

// Initialize empty array
int[] arr = new int[5]; // All elements are 0
```

## Accessing Array Elements
```java
int[] numbers = {10, 20, 30, 40, 50};

// Access element at index
int first = numbers[0];  // 10
int third = numbers[2];  // 30

// Modify element
numbers[1] = 25; // Changes 20 to 25

// Array length
int length = numbers.length; // 5
```

## Iterating Through Arrays

### Traditional For Loop
```java
int[] numbers = {1, 2, 3, 4, 5};
for (int i = 0; i < numbers.length; i++) {
    System.out.println(numbers[i]);
}
```

### Enhanced For Loop (For-Each)
```java
int[] numbers = {1, 2, 3, 4, 5};
for (int num : numbers) {
    System.out.println(num);
}
```

## Multi-Dimensional Arrays

### Two-Dimensional Array
```java
// Declaration
int[][] matrix;

// Initialization
int[][] matrix = {
    {1, 2, 3},
    {4, 5, 6},
    {7, 8, 9}
};

// With new keyword
int[][] matrix = new int[3][3]; // 3x3 matrix

// Jagged array (different row sizes)
int[][] jagged = {
    {1, 2},
    {3, 4, 5},
    {6, 7, 8, 9}
};
```

### Accessing 2D Array Elements
```java
int[][] matrix = {
    {1, 2, 3},
    {4, 5, 6}
};

int value = matrix[0][1]; // 2 (row 0, column 1)
matrix[1][2] = 10; // Modify element

// Iterating 2D array
for (int i = 0; i < matrix.length; i++) {
    for (int j = 0; j < matrix[i].length; j++) {
        System.out.print(matrix[i][j] + " ");
    }
    System.out.println();
}
```

## Common Array Operations

### Finding Maximum/Minimum
```java
int[] numbers = {5, 2, 8, 1, 9};
int max = numbers[0];
int min = numbers[0];

for (int num : numbers) {
    if (num > max) max = num;
    if (num < min) min = num;
}
```

### Sum of Elements
```java
int[] numbers = {1, 2, 3, 4, 5};
int sum = 0;
for (int num : numbers) {
    sum += num;
}
```

### Searching
```java
int[] numbers = {1, 2, 3, 4, 5};
int target = 3;
int index = -1;

for (int i = 0; i < numbers.length; i++) {
    if (numbers[i] == target) {
        index = i;
        break;
    }
}
```

### Copying Arrays
```java
int[] original = {1, 2, 3, 4, 5};

// Using System.arraycopy
int[] copy = new int[original.length];
System.arraycopy(original, 0, copy, 0, original.length);

// Using clone
int[] copy2 = original.clone();

// Using Arrays.copyOf (requires import java.util.Arrays)
int[] copy3 = Arrays.copyOf(original, original.length);
```

## Array Methods (java.util.Arrays)

```java
import java.util.Arrays;

int[] numbers = {5, 2, 8, 1, 9};

// Sort array
Arrays.sort(numbers); // {1, 2, 5, 8, 9}

// Binary search (array must be sorted)
int index = Arrays.binarySearch(numbers, 5);

// Fill array with value
Arrays.fill(numbers, 0); // All elements become 0

// Check equality
int[] arr1 = {1, 2, 3};
int[] arr2 = {1, 2, 3};
boolean equal = Arrays.equals(arr1, arr2); // true

// Convert to string
String str = Arrays.toString(numbers); // "[1, 2, 5, 8, 9]"
```

## Array Limitations
1. **Fixed Size**: Cannot change size after creation
2. **Same Type**: All elements must be same type
3. **No Built-in Methods**: Limited operations compared to Collections

## Array vs ArrayList
```java
// Array - fixed size
int[] arr = new int[5];

// ArrayList - dynamic size
ArrayList<Integer> list = new ArrayList<>();
list.add(1);
list.add(2);
list.remove(0);
```

## Common Array Patterns

### Reversing Array
```java
int[] numbers = {1, 2, 3, 4, 5};
for (int i = 0; i < numbers.length / 2; i++) {
    int temp = numbers[i];
    numbers[i] = numbers[numbers.length - 1 - i];
    numbers[numbers.length - 1 - i] = temp;
}
```

### Finding Duplicates
```java
int[] numbers = {1, 2, 3, 2, 4, 3};
for (int i = 0; i < numbers.length; i++) {
    for (int j = i + 1; j < numbers.length; j++) {
        if (numbers[i] == numbers[j]) {
            System.out.println("Duplicate: " + numbers[i]);
        }
    }
}
```

## Best Practices
1. Always check array bounds to avoid `ArrayIndexOutOfBoundsException`
2. Use enhanced for loop when index is not needed
3. Consider using Collections (ArrayList, etc.) for dynamic sizing
4. Initialize arrays properly to avoid null pointer exceptions
5. Use `Arrays` utility class for common operations


# Arrays (Java Implementation)

## Definition
An array is a collection of elements stored in contiguous memory locations. Each element can be accessed directly using its index position.

## Characteristics
- **Fixed or Dynamic Size**: Arrays can have a fixed size (static arrays) or can grow/shrink dynamically (dynamic arrays)
- **Index-based Access**: Elements are accessed using zero-based indexing
- **Contiguous Memory**: Elements are stored in consecutive memory locations
- **Homogeneous Data**: All elements in an array are typically of the same data type

## Operations and Time Complexity

| Operation | Time Complexity | Description |
|-----------|----------------|-------------|
| Access | O(1) | Access element at index |
| Search | O(n) | Linear search for an element |
| Insertion | O(n) | Insert at beginning/middle (requires shifting) |
| Deletion | O(n) | Delete at beginning/middle (requires shifting) |
| Append | O(1) amortized | Add element at the end (for dynamic arrays) |

## Space Complexity
- O(n) where n is the number of elements

## Advantages
- Fast random access by index
- Memory efficient (contiguous storage)
- Cache-friendly (better locality of reference)
- Simple to implement

## Disadvantages
- Fixed size in static arrays (wasted space or overflow)
- Insertion/deletion at arbitrary positions is expensive
- Requires shifting elements for insertions/deletions

## Use Cases
- Storing a collection of items where random access is needed
- Implementing other data structures (stacks, queues, hash tables)
- Matrix operations
- Image processing (pixel arrays)
- Database records

## Java Array Operations

### Basic Array Operations
```java
public class ArrayOperations {
    public static void main(String[] args) {
        // Create array
        int[] arr = {10, 20, 30, 40, 50};
        
        // Access element (O(1))
        int element = arr[2];  // Returns 30
        
        // Update element (O(1))
        arr[1] = 25;  // arr becomes [10, 25, 30, 40, 50]
        
        // Get array length
        int length = arr.length;  // Returns number of elements
        
        // Iterate through array
        for (int i = 0; i < arr.length; i++) {
            System.out.println(arr[i]);
        }
        
        // Enhanced for loop
        for (int num : arr) {
            System.out.println(num);
        }
    }
}
```

### Dynamic Array Implementation
```java
import java.util.Arrays;

public class DynamicArray {
    private int[] arr;
    private int size;
    private int capacity;
    
    public DynamicArray(int initialCapacity) {
        capacity = initialCapacity;
        size = 0;
        arr = new int[capacity];
    }
    
    public DynamicArray() {
        this(4);  // Default capacity
    }
    
    public int get(int index) {
        if (index < 0 || index >= size) {
            throw new IndexOutOfBoundsException("Index out of range");
        }
        return arr[index];
    }
    
    public void set(int index, int value) {
        if (index < 0 || index >= size) {
            throw new IndexOutOfBoundsException("Index out of range");
        }
        arr[index] = value;
    }
    
    public void pushBack(int value) {
        if (size >= capacity) {
            resize();
        }
        arr[size] = value;
        size++;
    }
    
    public int popBack() {
        if (size == 0) {
            throw new IllegalStateException("Array is empty");
        }
        size--;
        return arr[size];
    }
    
    public void insert(int index, int value) {
        if (index < 0 || index > size) {
            throw new IndexOutOfBoundsException("Index out of range");
        }
        if (size >= capacity) {
            resize();
        }
        // Shift elements to right
        for (int i = size; i > index; i--) {
            arr[i] = arr[i - 1];
        }
        arr[index] = value;
        size++;
    }
    
    public void delete(int index) {
        if (index < 0 || index >= size) {
            throw new IndexOutOfBoundsException("Index out of range");
        }
        // Shift elements to left
        for (int i = index; i < size - 1; i++) {
            arr[i] = arr[i + 1];
        }
        size--;
    }
    
    private void resize() {
        capacity *= 2;
        arr = Arrays.copyOf(arr, capacity);
    }
    
    public int size() {
        return size;
    }
    
    public boolean isEmpty() {
        return size == 0;
    }
}
```

### Multi-dimensional Arrays
```java
public class MultiDimensionalArrays {
    public static void main(String[] args) {
        // 2D Array (Matrix)
        int[][] matrix = {
            {1, 2, 3},
            {4, 5, 6},
            {7, 8, 9}
        };
        
        // Access element
        int element = matrix[1][2];  // Returns 6 (row 1, column 2)
        
        // Iterate through 2D array
        for (int i = 0; i < matrix.length; i++) {
            for (int j = 0; j < matrix[i].length; j++) {
                System.out.print(matrix[i][j] + " ");
            }
            System.out.println();
        }
        
        // 3D Array
        int[][][] cube = new int[3][3][3];
        
        // Initialize 3D array
        for (int i = 0; i < 3; i++) {
            for (int j = 0; j < 3; j++) {
                for (int k = 0; k < 3; k++) {
                    cube[i][j][k] = 0;
                }
            }
        }
    }
}
```

### Common Array Algorithms

#### Reverse Array
```java
public class ArrayAlgorithms {
    public static void reverseArray(int[] arr) {
        int left = 0;
        int right = arr.length - 1;
        while (left < right) {
            // Swap
            int temp = arr[left];
            arr[left] = arr[right];
            arr[right] = temp;
            left++;
            right--;
        }
    }
}
```

#### Find Maximum/Minimum
```java
    public static int findMax(int[] arr) {
        if (arr.length == 0) {
            throw new IllegalArgumentException("Array is empty");
        }
        int maxVal = arr[0];
        for (int i = 1; i < arr.length; i++) {
            if (arr[i] > maxVal) {
                maxVal = arr[i];
            }
        }
        return maxVal;
    }
    
    public static int findMin(int[] arr) {
        if (arr.length == 0) {
            throw new IllegalArgumentException("Array is empty");
        }
        int minVal = arr[0];
        for (int i = 1; i < arr.length; i++) {
            if (arr[i] < minVal) {
                minVal = arr[i];
            }
        }
        return minVal;
    }
}
```

#### Rotate Array
```java
    public static void rotateArray(int[] arr, int k) {
        int n = arr.length;
        k = k % n;  // Handle k > n
        
        // Reverse entire array
        reverse(arr, 0, n - 1);
        // Reverse first k elements
        reverse(arr, 0, k - 1);
        // Reverse remaining elements
        reverse(arr, k, n - 1);
    }
    
    private static void reverse(int[] arr, int start, int end) {
        while (start < end) {
            int temp = arr[start];
            arr[start] = arr[end];
            arr[end] = temp;
            start++;
            end--;
        }
    }
```

#### Array Rotation (Left)
```java
    public static int[] rotateLeft(int[] arr, int k) {
        int n = arr.length;
        k = k % n;
        int[] result = new int[n];
        
        for (int i = 0; i < n; i++) {
            result[i] = arr[(i + k) % n];
        }
        
        return result;
    }
```

#### Find Element in Array
```java
    public static int linearSearch(int[] arr, int target) {
        for (int i = 0; i < arr.length; i++) {
            if (arr[i] == target) {
                return i;
            }
        }
        return -1;  // Not found
    }
```

#### Copy Array
```java
    // Using System.arraycopy
    public static int[] copyArray(int[] source) {
        int[] destination = new int[source.length];
        System.arraycopy(source, 0, destination, 0, source.length);
        return destination;
    }
    
    // Using Arrays.copyOf
    public static int[] copyArray2(int[] source) {
        return Arrays.copyOf(source, source.length);
    }
```

## Java ArrayList (Dynamic Array)

```java
import java.util.ArrayList;

public class ArrayListExample {
    public static void main(String[] args) {
        // Create ArrayList
        ArrayList<Integer> list = new ArrayList<>();
        
        // Add elements
        list.add(10);
        list.add(20);
        list.add(30);
        
        // Access element
        int element = list.get(1);  // Returns 20
        
        // Update element
        list.set(1, 25);  // Set index 1 to 25
        
        // Insert at index
        list.add(1, 15);  // Insert 15 at index 1
        
        // Remove element
        list.remove(0);  // Remove element at index 0
        list.remove(Integer.valueOf(20));  // Remove first occurrence of 20
        
        // Size
        int size = list.size();
        
        // Check if empty
        boolean isEmpty = list.isEmpty();
        
        // Check if contains
        boolean contains = list.contains(30);
        
        // Clear
        list.clear();
    }
}
```

## Variations
- **Static Array**: Fixed size, allocated at compile time (int[] arr = new int[10])
- **Dynamic Array**: Size can change at runtime (ArrayList, Vector)
- **Multi-dimensional Array**: Arrays of arrays (2D, 3D, etc.)

## Common Array Problems
- Two Sum
- Maximum Subarray Sum (Kadane's Algorithm)
- Rotate Array
- Container With Most Water
- Trapping Rain Water
- Product of Array Except Self
- Merge Sorted Arrays
- Find Peak Element
- Search in Rotated Sorted Array
- Best Time to Buy and Sell Stock

# Sorting Algorithms (Java Implementation)

## Definition
Sorting is the process of arranging data in a particular order (ascending or descending). Sorting algorithms are fundamental algorithms used to organize data efficiently.

## Classification

### By Stability
- **Stable**: Maintains relative order of equal elements (Merge Sort, Bubble Sort, Insertion Sort, Counting Sort)
- **Unstable**: May change relative order of equal elements (Quick Sort, Heap Sort, Selection Sort)

### By Time Complexity
- **O(n log n)**: Merge Sort, Heap Sort, Quick Sort (average)
- **O(n²)**: Bubble Sort, Selection Sort, Insertion Sort
- **O(n)**: Counting Sort, Radix Sort, Bucket Sort (with restrictions)

### By Space Complexity
- **In-place**: O(1) extra space (Quick Sort, Heap Sort, Bubble Sort, Selection Sort, Insertion Sort)
- **Not in-place**: O(n) extra space (Merge Sort, Counting Sort, Radix Sort)

## Comparison-Based Sorts

### 1. Bubble Sort
**Description**: Repeatedly steps through the list, compares adjacent elements and swaps them if they are in wrong order.

**Time Complexity**: 
- Best: O(n) - already sorted
- Average: O(n²)
- Worst: O(n²) - reverse sorted

**Space Complexity**: O(1)

**Java Implementation**:
```java
public class BubbleSort {
    public static void bubbleSort(int[] arr) {
        int n = arr.length;
        for (int i = 0; i < n; i++) {
            boolean swapped = false;
            for (int j = 0; j < n - i - 1; j++) {
                if (arr[j] > arr[j + 1]) {
                    // Swap
                    int temp = arr[j];
                    arr[j] = arr[j + 1];
                    arr[j + 1] = temp;
                    swapped = true;
                }
            }
            if (!swapped) break;  // Early termination if already sorted
        }
    }
}
```

**Characteristics**:
- Stable
- In-place
- Adaptive (efficient for nearly sorted data)
- Simple to understand
- Poor performance on large datasets

---

### 2. Selection Sort
**Description**: Finds the minimum element from unsorted part and puts it at the beginning.

**Time Complexity**:
- Best: O(n²)
- Average: O(n²)
- Worst: O(n²)

**Space Complexity**: O(1)

**Java Implementation**:
```java
public class SelectionSort {
    public static void selectionSort(int[] arr) {
        int n = arr.length;
        for (int i = 0; i < n; i++) {
            int minIdx = i;
            for (int j = i + 1; j < n; j++) {
                if (arr[j] < arr[minIdx]) {
                    minIdx = j;
                }
            }
            // Swap
            int temp = arr[minIdx];
            arr[minIdx] = arr[i];
            arr[i] = temp;
        }
    }
}
```

**Characteristics**:
- Unstable
- In-place
- Not adaptive
- Fewer swaps than bubble sort
- Always O(n²) regardless of input

---

### 3. Insertion Sort
**Description**: Builds sorted array one item at a time, similar to sorting playing cards.

**Time Complexity**:
- Best: O(n) - already sorted
- Average: O(n²)
- Worst: O(n²) - reverse sorted

**Space Complexity**: O(1)

**Java Implementation**:
```java
public class InsertionSort {
    public static void insertionSort(int[] arr) {
        for (int i = 1; i < arr.length; i++) {
            int key = arr[i];
            int j = i - 1;
            
            // Move elements greater than key one position ahead
            while (j >= 0 && arr[j] > key) {
                arr[j + 1] = arr[j];
                j--;
            }
            arr[j + 1] = key;
        }
    }
}
```

**Characteristics**:
- Stable
- In-place
- Adaptive (very efficient for small or nearly sorted data)
- Simple implementation
- Efficient for small datasets (< 50 elements)

---

### 4. Merge Sort
**Description**: Divides array into two halves, sorts them, and merges the sorted halves.

**Time Complexity**: O(n log n) in all cases
- Best: O(n log n)
- Average: O(n log n)
- Worst: O(n log n)

**Space Complexity**: O(n)

**Java Implementation**:
```java
public class MergeSort {
    public static void mergeSort(int[] arr) {
        mergeSort(arr, 0, arr.length - 1);
    }
    
    private static void mergeSort(int[] arr, int left, int right) {
        if (left < right) {
            int mid = left + (right - left) / 2;
            
            mergeSort(arr, left, mid);
            mergeSort(arr, mid + 1, right);
            
            merge(arr, left, mid, right);
        }
    }
    
    private static void merge(int[] arr, int left, int mid, int right) {
        int n1 = mid - left + 1;
        int n2 = right - mid;
        
        int[] leftArr = new int[n1];
        int[] rightArr = new int[n2];
        
        System.arraycopy(arr, left, leftArr, 0, n1);
        System.arraycopy(arr, mid + 1, rightArr, 0, n2);
        
        int i = 0, j = 0, k = left;
        
        while (i < n1 && j < n2) {
            if (leftArr[i] <= rightArr[j]) {
                arr[k++] = leftArr[i++];
            } else {
                arr[k++] = rightArr[j++];
            }
        }
        
        while (i < n1) {
            arr[k++] = leftArr[i++];
        }
        
        while (j < n2) {
            arr[k++] = rightArr[j++];
        }
    }
}
```

**Characteristics**:
- Stable
- Not in-place (requires O(n) extra space)
- Guaranteed O(n log n) performance
- Good for linked lists
- Parallelizable

---

### 5. Quick Sort
**Description**: Picks a pivot element, partitions array around pivot, and recursively sorts sub-arrays.

**Time Complexity**:
- Best: O(n log n) - good pivot selection
- Average: O(n log n)
- Worst: O(n²) - poor pivot selection (already sorted)

**Space Complexity**: O(log n) average (recursion stack)

**Java Implementation**:
```java
public class QuickSort {
    public static void quickSort(int[] arr) {
        quickSort(arr, 0, arr.length - 1);
    }
    
    private static void quickSort(int[] arr, int low, int high) {
        if (low < high) {
            int pi = partition(arr, low, high);
            quickSort(arr, low, pi - 1);
            quickSort(arr, pi + 1, high);
        }
    }
    
    private static int partition(int[] arr, int low, int high) {
        int pivot = arr[high];
        int i = low - 1;
        
        for (int j = low; j < high; j++) {
            if (arr[j] < pivot) {
                i++;
                swap(arr, i, j);
            }
        }
        swap(arr, i + 1, high);
        return i + 1;
    }
    
    private static void swap(int[] arr, int i, int j) {
        int temp = arr[i];
        arr[i] = arr[j];
        arr[j] = temp;
    }
}
```

**Characteristics**:
- Unstable (typically)
- In-place
- Fast in practice (good average case)
- Cache-friendly
- Sensitive to pivot selection

**Pivot Selection Strategies**:
- First element (bad for sorted arrays)
- Last element (bad for reverse sorted)
- Median of three (better)
- Random (good average case)

---

### 6. Heap Sort
**Description**: Uses heap data structure to sort elements.

**Time Complexity**: O(n log n) in all cases
- Best: O(n log n)
- Average: O(n log n)
- Worst: O(n log n)

**Space Complexity**: O(1)

**Java Implementation**:
```java
public class HeapSort {
    public static void heapSort(int[] arr) {
        int n = arr.length;
        
        // Build max heap
        for (int i = n / 2 - 1; i >= 0; i--) {
            heapify(arr, n, i);
        }
        
        // Extract elements one by one
        for (int i = n - 1; i > 0; i--) {
            // Move root to end
            swap(arr, 0, i);
            
            // Heapify reduced heap
            heapify(arr, i, 0);
        }
    }
    
    private static void heapify(int[] arr, int n, int i) {
        int largest = i;
        int left = 2 * i + 1;
        int right = 2 * i + 2;
        
        if (left < n && arr[left] > arr[largest]) {
            largest = left;
        }
        
        if (right < n && arr[right] > arr[largest]) {
            largest = right;
        }
        
        if (largest != i) {
            swap(arr, i, largest);
            heapify(arr, n, largest);
        }
    }
    
    private static void swap(int[] arr, int i, int j) {
        int temp = arr[i];
        arr[i] = arr[j];
        arr[j] = temp;
    }
}
```

**Characteristics**:
- Unstable
- In-place
- Guaranteed O(n log n)
- Not adaptive
- Poor cache performance

## Non-Comparison-Based Sorts

### 7. Counting Sort
**Description**: Sorts by counting occurrences of each unique element.

**Time Complexity**: O(n + k) where k is range of input
**Space Complexity**: O(k)

**Java Implementation**:
```java
public class CountingSort {
    public static void countingSort(int[] arr, int maxVal) {
        int n = arr.length;
        int[] count = new int[maxVal + 1];
        int[] output = new int[n];
        
        // Count occurrences
        for (int num : arr) {
            count[num]++;
        }
        
        // Cumulative count
        for (int i = 1; i <= maxVal; i++) {
            count[i] += count[i - 1];
        }
        
        // Build output array
        for (int i = n - 1; i >= 0; i--) {
            output[count[arr[i]] - 1] = arr[i];
            count[arr[i]]--;
        }
        
        System.arraycopy(output, 0, arr, 0, n);
    }
}
```

**Limitations**: 
- Only works for integers
- Requires range to be known and small
- Not comparison-based

---

### 8. Radix Sort
**Description**: Sorts by processing digits from least significant to most significant.

**Time Complexity**: O(d × (n + k)) where d is number of digits, k is base
**Space Complexity**: O(n + k)

**Java Implementation**:
```java
public class RadixSort {
    public static void radixSort(int[] arr) {
        int max = getMax(arr);
        
        // Sort by each digit
        for (int exp = 1; max / exp > 0; exp *= 10) {
            countingSortByDigit(arr, exp);
        }
    }
    
    private static void countingSortByDigit(int[] arr, int exp) {
        int n = arr.length;
        int[] output = new int[n];
        int[] count = new int[10];
        
        for (int num : arr) {
            count[(num / exp) % 10]++;
        }
        
        for (int i = 1; i < 10; i++) {
            count[i] += count[i - 1];
        }
        
        for (int i = n - 1; i >= 0; i--) {
            output[count[(arr[i] / exp) % 10] - 1] = arr[i];
            count[(arr[i] / exp) % 10]--;
        }
        
        System.arraycopy(output, 0, arr, 0, n);
    }
    
    private static int getMax(int[] arr) {
        int max = arr[0];
        for (int num : arr) {
            if (num > max) max = num;
        }
        return max;
    }
}
```

---

## Algorithm Comparison Table

| Algorithm | Best | Average | Worst | Space | Stable | In-place |
|-----------|------|---------|-------|-------|--------|----------|
| Bubble Sort | O(n) | O(n²) | O(n²) | O(1) | Yes | Yes |
| Selection Sort | O(n²) | O(n²) | O(n²) | O(1) | No | Yes |
| Insertion Sort | O(n) | O(n²) | O(n²) | O(1) | Yes | Yes |
| Merge Sort | O(n log n) | O(n log n) | O(n log n) | O(n) | Yes | No |
| Quick Sort | O(n log n) | O(n log n) | O(n²) | O(log n) | No | Yes |
| Heap Sort | O(n log n) | O(n log n) | O(n log n) | O(1) | No | Yes |
| Counting Sort | O(n + k) | O(n + k) | O(n + k) | O(k) | Yes | No |
| Radix Sort | O(d × n) | O(d × n) | O(d × n) | O(n + k) | Yes | No |

## When to Use Which?

### Small Arrays (< 50 elements)
- **Insertion Sort**: Simple, stable, adaptive, efficient for small data

### Medium Arrays (50-1000 elements)
- **Quick Sort**: Fast average case, in-place, cache-friendly
- **Merge Sort**: If stability is required

### Large Arrays (> 1000 elements)
- **Quick Sort**: Best average performance
- **Merge Sort**: If guaranteed O(n log n) needed
- **Heap Sort**: If guaranteed O(n log n) and in-place needed

### Special Cases
- **Nearly Sorted**: Insertion Sort or Bubble Sort (adaptive)
- **Integers with Small Range**: Counting Sort or Radix Sort
- **Linked Lists**: Merge Sort (no random access needed)
- **External Sorting**: Merge Sort (handles large datasets on disk)

## Common Problems
- Sort an array
- Kth largest/smallest element
- Merge two sorted arrays
- Count inversions
- Sort by frequency
- Custom comparator sorting
- Sort colors (Dutch National Flag)

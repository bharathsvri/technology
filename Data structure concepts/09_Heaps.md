# Heaps (Java Implementation)

## Definition
A heap is a specialized tree-based data structure that satisfies the heap property. It is essentially an almost complete binary tree (complete at all levels except possibly the last level) that maintains ordering between parent and child nodes.

## Characteristics
- **Complete Binary Tree**: All levels filled except possibly the last (filled left to right)
- **Heap Property**: Parent-child ordering maintained
- **Partially Sorted**: Not fully sorted, but maintains ordering property
- **Efficient Priority Operations**: Fast min/max retrieval
- **Array Representation**: Typically stored in array for efficiency

## Types of Heaps

### 1. Min Heap
- Parent ≤ Children
- Root is minimum element
- Smallest element always at root

### 2. Max Heap
- Parent ≥ Children
- Root is maximum element
- Largest element always at root

### 3. Binary Heap
- Binary tree structure
- Most common implementation

### 4. Binomial Heap
- Collection of binomial trees
- More efficient merge operation

### 5. Fibonacci Heap
- Advanced heap structure
- Amortized O(1) insert, O(log n) extract-min

## Heap Property

### Min Heap Property
```
For every node i:
  value[i] ≤ value[leftChild(i)]
  value[i] ≤ value[rightChild(i)]
```

### Max Heap Property
```
For every node i:
  value[i] ≥ value[leftChild(i)]
  value[i] ≥ value[rightChild(i)]
```

## Array Representation

### Index Mapping
- Root at index 0
- For node at index `i`:
  - Parent: `(i - 1) / 2`
  - Left Child: `2i + 1`
  - Right Child: `2i + 2`

### Example
```
Max Heap:
        50
       /  \
      30   40
     / \   / \
    10 20 35 38

Array: [50, 30, 40, 10, 20, 35, 38]
Index:  0   1   2   3   4   5   6
```

## Operations

### Core Operations
1. **Insert**: Add element and maintain heap property
2. **Extract Min/Max**: Remove and return root element
3. **Peek**: View root without removing
4. **Heapify**: Convert array to heap
5. **Delete**: Remove specific element
6. **Decrease/Increase Key**: Modify element priority

## Time Complexity

| Operation | Time Complexity | Description |
|-----------|----------------|-------------|
| Insert | O(log n) | Add element and bubble up |
| Extract Min/Max | O(log n) | Remove root and heapify down |
| Peek | O(1) | Access root element |
| Heapify | O(n) | Build heap from array |
| Delete | O(log n) | Remove and maintain heap |
| Decrease/Increase Key | O(log n) | Modify and bubble up/down |

## Space Complexity
- O(n) where n is the number of elements
- Array storage: O(n)
- Recursion stack: O(log n) for heapify operations

## Java Implementation

### Min Heap Implementation
```java
import java.util.*;

public class MinHeap {
    private List<Integer> heap;
    
    public MinHeap() {
        heap = new ArrayList<>();
    }
    
    public MinHeap(int[] arr) {
        heap = new ArrayList<>();
        for (int num : arr) {
            heap.add(num);
        }
        buildHeap();
    }
    
    private int parent(int i) {
        return (i - 1) / 2;
    }
    
    private int leftChild(int i) {
        return 2 * i + 1;
    }
    
    private int rightChild(int i) {
        return 2 * i + 2;
    }
    
    private void swap(int i, int j) {
        int temp = heap.get(i);
        heap.set(i, heap.get(j));
        heap.set(j, temp);
    }
    
    // Insert element
    public void insert(int value) {
        heap.add(value);
        bubbleUp(heap.size() - 1);
    }
    
    // Bubble up to maintain heap property
    private void bubbleUp(int index) {
        while (index > 0 && heap.get(parent(index)) > heap.get(index)) {
            swap(index, parent(index));
            index = parent(index);
        }
    }
    
    // Extract minimum
    public int extractMin() {
        if (heap.isEmpty()) {
            throw new IllegalStateException("Heap is empty");
        }
        
        int min = heap.get(0);
        heap.set(0, heap.get(heap.size() - 1));
        heap.remove(heap.size() - 1);
        
        if (!heap.isEmpty()) {
            heapifyDown(0);
        }
        
        return min;
    }
    
    // Heapify down
    private void heapifyDown(int index) {
        int smallest = index;
        int left = leftChild(index);
        int right = rightChild(index);
        
        if (left < heap.size() && heap.get(left) < heap.get(smallest)) {
            smallest = left;
        }
        
        if (right < heap.size() && heap.get(right) < heap.get(smallest)) {
            smallest = right;
        }
        
        if (smallest != index) {
            swap(index, smallest);
            heapifyDown(smallest);
        }
    }
    
    // Build heap from array
    private void buildHeap() {
        for (int i = heap.size() / 2 - 1; i >= 0; i--) {
            heapifyDown(i);
        }
    }
    
    // Peek at root
    public int peek() {
        if (heap.isEmpty()) {
            throw new IllegalStateException("Heap is empty");
        }
        return heap.get(0);
    }
    
    public int size() {
        return heap.size();
    }
    
    public boolean isEmpty() {
        return heap.isEmpty();
    }
}
```

### Max Heap Implementation
```java
public class MaxHeap {
    private List<Integer> heap;
    
    public MaxHeap() {
        heap = new ArrayList<>();
    }
    
    public MaxHeap(int[] arr) {
        heap = new ArrayList<>();
        for (int num : arr) {
            heap.add(num);
        }
        buildHeap();
    }
    
    private int parent(int i) {
        return (i - 1) / 2;
    }
    
    private int leftChild(int i) {
        return 2 * i + 1;
    }
    
    private int rightChild(int i) {
        return 2 * i + 2;
    }
    
    private void swap(int i, int j) {
        int temp = heap.get(i);
        heap.set(i, heap.get(j));
        heap.set(j, temp);
    }
    
    public void insert(int value) {
        heap.add(value);
        bubbleUp(heap.size() - 1);
    }
    
    private void bubbleUp(int index) {
        while (index > 0 && heap.get(parent(index)) < heap.get(index)) {
            swap(index, parent(index));
            index = parent(index);
        }
    }
    
    public int extractMax() {
        if (heap.isEmpty()) {
            throw new IllegalStateException("Heap is empty");
        }
        
        int max = heap.get(0);
        heap.set(0, heap.get(heap.size() - 1));
        heap.remove(heap.size() - 1);
        
        if (!heap.isEmpty()) {
            heapifyDown(0);
        }
        
        return max;
    }
    
    private void heapifyDown(int index) {
        int largest = index;
        int left = leftChild(index);
        int right = rightChild(index);
        
        if (left < heap.size() && heap.get(left) > heap.get(largest)) {
            largest = left;
        }
        
        if (right < heap.size() && heap.get(right) > heap.get(largest)) {
            largest = right;
        }
        
        if (largest != index) {
            swap(index, largest);
            heapifyDown(largest);
        }
    }
    
    private void buildHeap() {
        for (int i = heap.size() / 2 - 1; i >= 0; i--) {
            heapifyDown(i);
        }
    }
    
    public int peek() {
        if (heap.isEmpty()) {
            throw new IllegalStateException("Heap is empty");
        }
        return heap.get(0);
    }
    
    public int size() {
        return heap.size();
    }
    
    public boolean isEmpty() {
        return heap.isEmpty();
    }
}
```

### Using Java PriorityQueue
```java
import java.util.*;

public class PriorityQueueExample {
    public static void main(String[] args) {
        // Min Heap (default)
        PriorityQueue<Integer> minHeap = new PriorityQueue<>();
        minHeap.offer(10);
        minHeap.offer(5);
        minHeap.offer(15);
        System.out.println(minHeap.poll());  // 5 (minimum)
        
        // Max Heap (using reverse order)
        PriorityQueue<Integer> maxHeap = new PriorityQueue<>(Collections.reverseOrder());
        maxHeap.offer(10);
        maxHeap.offer(5);
        maxHeap.offer(15);
        System.out.println(maxHeap.poll());  // 15 (maximum)
    }
}
```

## Heap Operations Details

### Insert Operation
1. Add element at end of array (last position in tree)
2. Compare with parent
3. If violates heap property, swap with parent
4. Continue bubbling up until heap property satisfied
5. **Time**: O(log n)

### Extract Min/Max Operation
1. Remove root element
2. Move last element to root
3. Compare with children
4. If violates heap property, swap with larger/smaller child
5. Continue bubbling down until heap property satisfied
6. **Time**: O(log n)

### Heapify Operation
- Convert unsorted array to heap
- Start from last parent node (n/2 - 1)
- Bubble down each node
- **Time**: O(n) (better than O(n log n))

## Advantages
- Efficient priority queue operations
- O(1) access to min/max element
- O(log n) insert and extract operations
- O(n) heap construction
- Simple array-based implementation
- Cache-friendly (array representation)

## Disadvantages
- Not suitable for arbitrary access
- Not fully sorted (only partial ordering)
- Slow search operation (O(n))
- No efficient way to find arbitrary element

## Use Cases
- Priority Queues
- Heap Sort algorithm
- Dijkstra's algorithm (shortest path)
- Prim's algorithm (MST)
- Top K problems (K largest/smallest elements)
- Merge K sorted lists
- Event-driven simulation
- Task scheduling
- Huffman coding

## Example Applications

### Priority Queue
```java
// Min heap for priority queue
PriorityQueue<String> tasks = new PriorityQueue<>();
tasks.offer("High Priority Task");
tasks.offer("Medium Priority Task");
tasks.offer("Low Priority Task");
String nextTask = tasks.poll();  // Get highest priority
```

### Top K Elements
```java
// Find K largest elements using min heap
public static List<Integer> findKLargest(int[] nums, int k) {
    PriorityQueue<Integer> minHeap = new PriorityQueue<>();
    
    for (int num : nums) {
        if (minHeap.size() < k) {
            minHeap.offer(num);
        } else if (num > minHeap.peek()) {
            minHeap.poll();
            minHeap.offer(num);
        }
    }
    
    List<Integer> result = new ArrayList<>(minHeap);
    Collections.reverse(result);
    return result;
}
```

### Heap Sort
```java
public static void heapSort(int[] arr) {
    int n = arr.length;
    
    // Build max heap
    for (int i = n / 2 - 1; i >= 0; i--) {
        heapify(arr, n, i);
    }
    
    // Extract elements one by one
    for (int i = n - 1; i > 0; i--) {
        // Move root to end
        int temp = arr[0];
        arr[0] = arr[i];
        arr[i] = temp;
        
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
        int temp = arr[i];
        arr[i] = arr[largest];
        arr[largest] = temp;
        
        heapify(arr, n, largest);
    }
}
```

## Heap vs Other Structures

### Heap vs BST
- Heap: Partial ordering, faster insert/extract
- BST: Full ordering, slower but supports search

### Heap vs Sorted Array
- Heap: O(log n) insert, O(log n) extract
- Sorted Array: O(n) insert, O(1) extract

## Common Problems
- Kth Largest Element in Array
- Merge K Sorted Lists
- Find Median from Data Stream
- Top K Frequent Elements
- Meeting Rooms II
- Design Twitter Feed
- Sliding Window Maximum
- Employee Free Time

## Implementation Tips
- Use 0-indexed arrays for simplicity
- Implement heapify functions carefully
- Handle edge cases (empty heap, single element)
- Consider max vs min heap based on problem
- Use heap for priority queue operations
- Use Java's PriorityQueue for practical applications

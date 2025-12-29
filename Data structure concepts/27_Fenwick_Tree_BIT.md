# Fenwick Tree (Binary Indexed Tree - BIT)

## Definition
A Fenwick Tree (Binary Indexed Tree) is a data structure that supports efficient range queries and point updates. It's simpler than Segment Tree and uses less memory.

## Properties
- **Space**: O(n) (more efficient than Segment Tree)
- **Range Query**: O(log n)
- **Point Update**: O(log n)
- **Build**: O(n log n)
- **Based on**: Binary representation and bit manipulation

## Key Concept
Each index i stores sum of elements from position (i - 2^r + 1) to i, where r is the position of the last set bit in i.

## Operations

### 1. Get Least Significant Set Bit
```java
private int getLSB(int i) {
    return i & -i;  // Also: i & (~i + 1)
}
```

### 2. Point Update
Add value to element at index i.

### 3. Range Query
Query sum from index 1 to index i.

## Java Implementation

```java
public class FenwickTree {
    private int[] tree;
    private int n;
    
    public FenwickTree(int size) {
        n = size;
        tree = new int[n + 1];  // 1-indexed
    }
    
    // Build from array
    public FenwickTree(int[] arr) {
        n = arr.length;
        tree = new int[n + 1];
        for (int i = 0; i < n; i++) {
            update(i, arr[i]);
        }
    }
    
    // Update: add value to element at index (0-indexed)
    public void update(int index, int delta) {
        index++;  // Convert to 1-indexed
        while (index <= n) {
            tree[index] += delta;
            index += index & -index;  // Move to parent
        }
    }
    
    // Query: get sum from index 0 to index (0-indexed)
    public int query(int index) {
        index++;  // Convert to 1-indexed
        int sum = 0;
        while (index > 0) {
            sum += tree[index];
            index -= index & -index;  // Move to parent
        }
        return sum;
    }
    
    // Range query: sum from left to right (both inclusive, 0-indexed)
    public int rangeQuery(int left, int right) {
        return query(right) - query(left - 1);
    }
    
    // Get value at index (0-indexed)
    public int get(int index) {
        return rangeQuery(index, index);
    }
    
    // Set value at index (0-indexed)
    public void set(int index, int value) {
        int current = get(index);
        update(index, value - current);
    }
}
```

## 2D Fenwick Tree

```java
public class FenwickTree2D {
    private int[][] tree;
    private int rows;
    private int cols;
    
    public FenwickTree2D(int rows, int cols) {
        this.rows = rows;
        this.cols = cols;
        tree = new int[rows + 1][cols + 1];
    }
    
    public void update(int row, int col, int delta) {
        row++;
        col++;
        for (int i = row; i <= rows; i += i & -i) {
            for (int j = col; j <= cols; j += j & -j) {
                tree[i][j] += delta;
            }
        }
    }
    
    public int query(int row, int col) {
        row++;
        col++;
        int sum = 0;
        for (int i = row; i > 0; i -= i & -i) {
            for (int j = col; j > 0; j -= j & -j) {
                sum += tree[i][j];
            }
        }
        return sum;
    }
    
    // Query sum in rectangle from (r1, c1) to (r2, c2)
    public int rangeQuery(int r1, int c1, int r2, int c2) {
        return query(r2, c2) - query(r1 - 1, c2) 
             - query(r2, c1 - 1) + query(r1 - 1, c1 - 1);
    }
}
```

## Time Complexity

| Operation | Time Complexity |
|-----------|----------------|
| Update | O(log n) |
| Query | O(log n) |
| Range Query | O(log n) |
| Build | O(n log n) |

## Space Complexity
- O(n) for 1D
- O(rows × cols) for 2D

## Advantages over Segment Tree
- Simpler implementation
- Less memory (O(n) vs O(4n))
- Faster constant factors
- Easier to code

## Disadvantages
- Less flexible than Segment Tree
- Only supports prefix sum queries efficiently
- Harder to extend to other operations

## Common Problems
- Range Sum Query - Mutable
- Count of Smaller Numbers After Self
- Reverse Pairs
- Count of Range Sum
- Maximum Sum of 3 Non-Overlapping Subarrays
- Number of Ways to Split Array
- Range Sum Query 2D - Mutable


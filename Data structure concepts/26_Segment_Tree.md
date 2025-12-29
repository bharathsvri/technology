# Segment Tree

## Definition
A Segment Tree is a tree data structure used for storing information about intervals (segments). It allows querying which of the stored segments contain a given point, or finding the minimum/maximum/sum in a range efficiently.

## Properties
- **Full Binary Tree**: Complete binary tree structure
- **Range Queries**: O(log n) for range queries
- **Point Updates**: O(log n) for point updates
- **Range Updates**: O(log n) with lazy propagation
- **Space**: O(4*n) in worst case

## Use Cases
- Range Sum Query
- Range Minimum/Maximum Query
- Range GCD/LCM Query
- Range Update + Point Query
- Range Update + Range Query (with lazy propagation)

## Java Implementation - Range Sum Query

```java
public class SegmentTree {
    private int[] tree;
    private int n;
    
    public SegmentTree(int[] arr) {
        n = arr.length;
        tree = new int[4 * n];
        build(arr, 0, 0, n - 1);
    }
    
    // Build segment tree
    private void build(int[] arr, int node, int start, int end) {
        if (start == end) {
            tree[node] = arr[start];
        } else {
            int mid = (start + end) / 2;
            build(arr, 2 * node + 1, start, mid);
            build(arr, 2 * node + 2, mid + 1, end);
            tree[node] = tree[2 * node + 1] + tree[2 * node + 2];
        }
    }
    
    // Update value at index
    public void update(int index, int value) {
        update(0, 0, n - 1, index, value);
    }
    
    private void update(int node, int start, int end, int index, int value) {
        if (start == end) {
            tree[node] = value;
        } else {
            int mid = (start + end) / 2;
            if (index <= mid) {
                update(2 * node + 1, start, mid, index, value);
            } else {
                update(2 * node + 2, mid + 1, end, index, value);
            }
            tree[node] = tree[2 * node + 1] + tree[2 * node + 2];
        }
    }
    
    // Range sum query
    public int query(int left, int right) {
        return query(0, 0, n - 1, left, right);
    }
    
    private int query(int node, int start, int end, int left, int right) {
        if (right < start || left > end) {
            return 0; // Out of range
        }
        if (left <= start && end <= right) {
            return tree[node]; // Completely within range
        }
        
        int mid = (start + end) / 2;
        int leftSum = query(2 * node + 1, start, mid, left, right);
        int rightSum = query(2 * node + 2, mid + 1, end, left, right);
        return leftSum + rightSum;
    }
}
```

## Range Minimum Query (RMQ)

```java
public class SegmentTreeRMQ {
    private int[] tree;
    private int n;
    
    public SegmentTreeRMQ(int[] arr) {
        n = arr.length;
        tree = new int[4 * n];
        build(arr, 0, 0, n - 1);
    }
    
    private void build(int[] arr, int node, int start, int end) {
        if (start == end) {
            tree[node] = arr[start];
        } else {
            int mid = (start + end) / 2;
            build(arr, 2 * node + 1, start, mid);
            build(arr, 2 * node + 2, mid + 1, end);
            tree[node] = Math.min(tree[2 * node + 1], tree[2 * node + 2]);
        }
    }
    
    public void update(int index, int value) {
        update(0, 0, n - 1, index, value);
    }
    
    private void update(int node, int start, int end, int index, int value) {
        if (start == end) {
            tree[node] = value;
        } else {
            int mid = (start + end) / 2;
            if (index <= mid) {
                update(2 * node + 1, start, mid, index, value);
            } else {
                update(2 * node + 2, mid + 1, end, index, value);
            }
            tree[node] = Math.min(tree[2 * node + 1], tree[2 * node + 2]);
        }
    }
    
    public int query(int left, int right) {
        return query(0, 0, n - 1, left, right);
    }
    
    private int query(int node, int start, int end, int left, int right) {
        if (right < start || left > end) {
            return Integer.MAX_VALUE;
        }
        if (left <= start && end <= right) {
            return tree[node];
        }
        int mid = (start + end) / 2;
        int leftMin = query(2 * node + 1, start, mid, left, right);
        int rightMin = query(2 * node + 2, mid + 1, end, left, right);
        return Math.min(leftMin, rightMin);
    }
}
```

## Lazy Propagation (Range Update)

```java
public class SegmentTreeLazy {
    private int[] tree;
    private int[] lazy;
    private int n;
    
    public SegmentTreeLazy(int[] arr) {
        n = arr.length;
        tree = new int[4 * n];
        lazy = new int[4 * n];
        build(arr, 0, 0, n - 1);
    }
    
    private void build(int[] arr, int node, int start, int end) {
        if (start == end) {
            tree[node] = arr[start];
        } else {
            int mid = (start + end) / 2;
            build(arr, 2 * node + 1, start, mid);
            build(arr, 2 * node + 2, mid + 1, end);
            tree[node] = tree[2 * node + 1] + tree[2 * node + 2];
        }
    }
    
    private void push(int node, int start, int end) {
        if (lazy[node] != 0) {
            tree[node] += lazy[node] * (end - start + 1);
            if (start != end) {
                lazy[2 * node + 1] += lazy[node];
                lazy[2 * node + 2] += lazy[node];
            }
            lazy[node] = 0;
        }
    }
    
    // Range update: add value to all elements in range [left, right]
    public void rangeUpdate(int left, int right, int value) {
        rangeUpdate(0, 0, n - 1, left, right, value);
    }
    
    private void rangeUpdate(int node, int start, int end, int left, int right, int value) {
        push(node, start, end);
        if (right < start || left > end) return;
        
        if (left <= start && end <= right) {
            lazy[node] += value;
            push(node, start, end);
            return;
        }
        
        int mid = (start + end) / 2;
        rangeUpdate(2 * node + 1, start, mid, left, right, value);
        rangeUpdate(2 * node + 2, mid + 1, end, left, right, value);
        push(2 * node + 1, start, mid);
        push(2 * node + 2, mid + 1, end);
        tree[node] = tree[2 * node + 1] + tree[2 * node + 2];
    }
    
    public int query(int left, int right) {
        return query(0, 0, n - 1, left, right);
    }
    
    private int query(int node, int start, int end, int left, int right) {
        push(node, start, end);
        if (right < start || left > end) return 0;
        if (left <= start && end <= right) return tree[node];
        
        int mid = (start + end) / 2;
        int leftSum = query(2 * node + 1, start, mid, left, right);
        int rightSum = query(2 * node + 2, mid + 1, end, left, right);
        return leftSum + rightSum;
    }
}
```

## Time Complexity

| Operation | Time Complexity |
|-----------|----------------|
| Build | O(n) |
| Point Update | O(log n) |
| Range Query | O(log n) |
| Range Update (lazy) | O(log n) |

## Space Complexity
- O(4*n) for tree array
- O(4*n) for lazy array (if using lazy propagation)

## Common Problems
- Range Sum Query - Mutable
- Range Minimum Query
- Range Maximum Query
- Range GCD Query
- Range Update + Point Query
- Range Update + Range Query
- Count of Smaller Numbers After Self
- Reverse Pairs
- My Calendar I/II/III


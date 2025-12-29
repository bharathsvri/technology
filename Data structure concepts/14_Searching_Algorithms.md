# Searching Algorithms (Java Implementation)

## Definition
Searching is the process of finding a particular element in a collection of data. The efficiency of searching algorithms varies based on whether the data is sorted and the data structure used.

## Types of Searching

### 1. Linear Search (Sequential Search)
**Description**: Checks each element sequentially until the target is found or all elements are checked.

**Algorithm**:
```
1. Start from first element
2. Compare with target
3. If match, return index
4. Otherwise, move to next element
5. Repeat until found or end of array
```

**Time Complexity**:
- Best: O(1) - element at first position
- Average: O(n)
- Worst: O(n) - element not present or at last position

**Space Complexity**: O(1)

**Java Implementation**:
```java
public class LinearSearch {
    // Iterative approach
    public static int linearSearch(int[] arr, int target) {
        for (int i = 0; i < arr.length; i++) {
            if (arr[i] == target) {
                return i;
            }
        }
        return -1;
    }
    
    // Recursive version
    public static int linearSearchRecursive(int[] arr, int target, int index) {
        if (index >= arr.length) {
            return -1;
        }
        if (arr[index] == target) {
            return index;
        }
        return linearSearchRecursive(arr, target, index + 1);
    }
    
    // Overloaded for easier use
    public static int linearSearchRecursive(int[] arr, int target) {
        return linearSearchRecursive(arr, target, 0);
    }
}
```

**Characteristics**:
- Works on unsorted arrays
- Simple to implement
- No preprocessing needed
- Inefficient for large datasets
- Can be optimized with sentinel values

**Optimizations**:
- **Sentinel Linear Search**: Place target at end to avoid boundary checks
- **Transposition**: Move found element closer to front for repeated searches
- **Move to Front**: Move found element to front

---

### 2. Binary Search
**Description**: Efficiently searches a sorted array by repeatedly dividing search interval in half.

**Prerequisites**: Array must be sorted

**Algorithm**:
```
1. Compare target with middle element
2. If match, return index
3. If target > middle, search right half
4. If target < middle, search left half
5. Repeat until found or search space exhausted
```

**Time Complexity**: O(log n)
- Best: O(1) - element at middle
- Average: O(log n)
- Worst: O(log n)

**Space Complexity**: 
- Iterative: O(1)
- Recursive: O(log n) for recursion stack

**Java Implementation**:
```java
public class BinarySearch {
    // Iterative approach
    public static int binarySearch(int[] arr, int target) {
        int left = 0;
        int right = arr.length - 1;
        
        while (left <= right) {
            int mid = left + (right - left) / 2;
            
            if (arr[mid] == target) {
                return mid;
            } else if (arr[mid] < target) {
                left = mid + 1;
            } else {
                right = mid - 1;
            }
        }
        
        return -1;
    }
    
    // Recursive approach
    public static int binarySearchRecursive(int[] arr, int target, int left, int right) {
        if (left > right) {
            return -1;
        }
        
        int mid = left + (right - left) / 2;
        
        if (arr[mid] == target) {
            return mid;
        } else if (arr[mid] < target) {
            return binarySearchRecursive(arr, target, mid + 1, right);
        } else {
            return binarySearchRecursive(arr, target, left, mid - 1);
        }
    }
    
    // Overloaded for easier use
    public static int binarySearchRecursive(int[] arr, int target) {
        return binarySearchRecursive(arr, target, 0, arr.length - 1);
    }
}
```

**Variations**:

#### Lower Bound (First occurrence)
```java
public static int lowerBound(int[] arr, int target) {
    int left = 0;
    int right = arr.length;
    
    while (left < right) {
        int mid = left + (right - left) / 2;
        if (arr[mid] < target) {
            left = mid + 1;
        } else {
            right = mid;
        }
    }
    
    return left;
}
```

#### Upper Bound (Last occurrence)
```java
public static int upperBound(int[] arr, int target) {
    int left = 0;
    int right = arr.length;
    
    while (left < right) {
        int mid = left + (right - left) / 2;
        if (arr[mid] <= target) {
            left = mid + 1;
        } else {
            right = mid;
        }
    }
    
    return left;
}
```

#### Search in Rotated Sorted Array
```java
public static int searchRotated(int[] arr, int target) {
    int left = 0;
    int right = arr.length - 1;
    
    while (left <= right) {
        int mid = left + (right - left) / 2;
        
        if (arr[mid] == target) {
            return mid;
        }
        
        // Left half is sorted
        if (arr[left] <= arr[mid]) {
            if (arr[left] <= target && target < arr[mid]) {
                right = mid - 1;
            } else {
                left = mid + 1;
            }
        } 
        // Right half is sorted
        else {
            if (arr[mid] < target && target <= arr[right]) {
                left = mid + 1;
            } else {
                right = mid - 1;
            }
        }
    }
    
    return -1;
}
```

**Characteristics**:
- Requires sorted array
- Very efficient (O(log n))
- Divide and conquer approach
- Can be applied to many variations

---

### 3. Ternary Search
**Description**: Divides search space into three parts instead of two.

**Algorithm**:
```
1. Divide array into three parts using two mid points
2. Compare target with both mid points
3. Determine which third contains target
4. Recursively search that third
```

**Time Complexity**: O(log₃ n) ≈ O(log n) (but more comparisons per iteration)

**Java Implementation**:
```java
public class TernarySearch {
    public static int ternarySearch(int[] arr, int target, int left, int right) {
        if (left > right) {
            return -1;
        }
        
        int mid1 = left + (right - left) / 3;
        int mid2 = right - (right - left) / 3;
        
        if (arr[mid1] == target) {
            return mid1;
        }
        if (arr[mid2] == target) {
            return mid2;
        }
        
        if (target < arr[mid1]) {
            return ternarySearch(arr, target, left, mid1 - 1);
        } else if (target > arr[mid2]) {
            return ternarySearch(arr, target, mid2 + 1, right);
        } else {
            return ternarySearch(arr, target, mid1 + 1, mid2 - 1);
        }
    }
    
    // Overloaded for easier use
    public static int ternarySearch(int[] arr, int target) {
        return ternarySearch(arr, target, 0, arr.length - 1);
    }
}
```

**Note**: Binary search is generally preferred (fewer comparisons per iteration)

---

### 4. Jump Search
**Description**: Searches by jumping ahead by fixed steps, then performing linear search in block.

**Algorithm**:
```
1. Jump ahead by √n steps
2. If jump exceeds array or element > target, go back
3. Perform linear search in previous block
```

**Time Complexity**: O(√n)
**Space Complexity**: O(1)

**Optimal Block Size**: √n

**Java Implementation**:
```java
public class JumpSearch {
    public static int jumpSearch(int[] arr, int target) {
        int n = arr.length;
        int step = (int) Math.sqrt(n);
        int prev = 0;
        
        // Find the block where element is present
        while (arr[Math.min(step, n) - 1] < target) {
            prev = step;
            step += (int) Math.sqrt(n);
            if (prev >= n) {
                return -1;
            }
        }
        
        // Linear search in the block
        while (arr[prev] < target) {
            prev++;
            if (prev == Math.min(step, n)) {
                return -1;
            }
        }
        
        if (arr[prev] == target) {
            return prev;
        }
        
        return -1;
    }
}
```

**Characteristics**:
- Works on sorted arrays
- Better than linear search, worse than binary search
- Useful when binary search is expensive (e.g., on tape drives)

---

### 5. Interpolation Search
**Description**: Improved binary search for uniformly distributed sorted arrays. Uses formula to guess position.

**Algorithm**:
```
1. Use formula to estimate position: pos = low + ((target - arr[low]) * (high - low)) / (arr[high] - arr[low])
2. Compare with estimated position
3. Adjust search range based on comparison
```

**Time Complexity**:
- Best: O(log log n) for uniformly distributed data
- Average: O(log log n)
- Worst: O(n) for non-uniform distribution

**Java Implementation**:
```java
public class InterpolationSearch {
    public static int interpolationSearch(int[] arr, int target) {
        int left = 0;
        int right = arr.length - 1;
        
        while (left <= right && arr[left] <= target && target <= arr[right]) {
            if (left == right) {
                if (arr[left] == target) {
                    return left;
                }
                return -1;
            }
            
            // Formula for position estimation
            int pos = left + ((target - arr[left]) * (right - left)) / 
                      (arr[right] - arr[left]);
            
            if (arr[pos] == target) {
                return pos;
            } else if (arr[pos] < target) {
                left = pos + 1;
            } else {
                right = pos - 1;
            }
        }
        
        return -1;
    }
}
```

**Characteristics**:
- Requires uniformly distributed sorted array
- Very fast for uniform distribution
- Performs poorly for non-uniform distribution

---

### 6. Exponential Search
**Description**: Finds range where element might be, then applies binary search.

**Algorithm**:
```
1. Start with subarray of size 1
2. Double size until element at that index exceeds target
3. Apply binary search in the found range
```

**Time Complexity**: O(log n)

**Java Implementation**:
```java
public class ExponentialSearch {
    public static int exponentialSearch(int[] arr, int target) {
        int n = arr.length;
        
        if (arr[0] == target) {
            return 0;
        }
        
        int i = 1;
        while (i < n && arr[i] <= target) {
            i *= 2;
        }
        
        // Binary search in the found range
        return binarySearch(arr, target, i / 2, Math.min(i, n - 1));
    }
    
    private static int binarySearch(int[] arr, int target, int left, int right) {
        while (left <= right) {
            int mid = left + (right - left) / 2;
            if (arr[mid] == target) {
                return mid;
            } else if (arr[mid] < target) {
                left = mid + 1;
            } else {
                right = mid - 1;
            }
        }
        return -1;
    }
}
```

**Use Cases**: 
- Useful for unbounded arrays
- When target is near beginning of array

---

## Searching in Different Data Structures

### Binary Search Tree (BST)
**Time Complexity**: 
- Average: O(log n)
- Worst: O(n) if tree is unbalanced

```java
class TreeNode {
    int val;
    TreeNode left;
    TreeNode right;
    TreeNode(int val) { this.val = val; }
}

public class BSTSearch {
    public TreeNode searchBST(TreeNode root, int target) {
        if (root == null || root.val == target) {
            return root;
        }
        
        if (root.val < target) {
            return searchBST(root.right, target);
        }
        
        return searchBST(root.left, target);
    }
}
```

### Hash Table
**Time Complexity**: O(1) average, O(n) worst case

```java
import java.util.HashMap;

public class HashTableSearch {
    public boolean searchHashTable(HashMap<Integer, Integer> hashMap, int key) {
        return hashMap.containsKey(key);  // O(1) average
    }
}
```

### Trie (Prefix Tree)
**Time Complexity**: O(m) where m is length of string

```java
class TrieNode {
    TrieNode[] children;
    boolean isEndOfWord;
    
    TrieNode() {
        children = new TrieNode[26];  // For lowercase English letters
        isEndOfWord = false;
    }
}

public class TrieSearch {
    public boolean searchTrie(TrieNode root, String word) {
        TrieNode node = root;
        for (char c : word.toCharArray()) {
            int index = c - 'a';
            if (node.children[index] == null) {
                return false;
            }
            node = node.children[index];
        }
        return node.isEndOfWord;
    }
}
```

## Algorithm Comparison

| Algorithm | Time Complexity | Space Complexity | Prerequisites |
|-----------|----------------|------------------|---------------|
| Linear Search | O(n) | O(1) | None |
| Binary Search | O(log n) | O(1)/O(log n) | Sorted array |
| Ternary Search | O(log₃ n) | O(1)/O(log n) | Sorted array |
| Jump Search | O(√n) | O(1) | Sorted array |
| Interpolation Search | O(log log n) | O(1) | Sorted, uniform distribution |
| Exponential Search | O(log n) | O(1)/O(log n) | Sorted array |

## Common Problems
- Binary Search
- Search in Rotated Sorted Array
- Find First and Last Position
- Search in 2D Matrix
- Find Peak Element
- Search Insert Position
- Find Minimum in Rotated Sorted Array
- Kth Smallest/Largest Element

## Best Practices
- Use linear search for small unsorted arrays
- Use binary search for sorted arrays (most common)
- Consider hash table for O(1) lookups when possible
- Choose interpolation search for uniformly distributed sorted data
- Use exponential search for unbounded arrays

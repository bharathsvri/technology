# Recursion

## Definition
Recursion is a programming technique where a function calls itself to solve a problem by breaking it down into smaller, similar subproblems.

## Key Components

1. **Base Case**: Condition that stops recursion (prevents infinite loop)
2. **Recursive Case**: Function calls itself with modified parameters
3. **Recursive Call**: The function calling itself

## Characteristics
- **Self-Referential**: Function calls itself
- **Divide and Conquer**: Breaks problem into smaller subproblems
- **Stack Usage**: Uses call stack to track function calls
- **Memory Overhead**: Each call uses stack space

## How Recursion Works

```
Function Call Stack:
- Each recursive call pushes to stack
- Base case reached → start returning
- Values pop from stack in reverse order
```

## Types of Recursion

### 1. Direct Recursion
Function calls itself directly.

```python
def factorial(n):
    if n <= 1:  # Base case
        return 1
    return n * factorial(n - 1)  # Recursive case
```

### 2. Indirect Recursion
Function A calls function B, which calls function A.

```python
def function_a(n):
    if n <= 0:
        return
    print("A:", n)
    function_b(n - 1)

def function_b(n):
    if n <= 0:
        return
    print("B:", n)
    function_a(n - 1)
```

### 3. Tail Recursion
Recursive call is the last operation in function.

```python
def factorial_tail(n, acc=1):
    if n <= 1:
        return acc
    return factorial_tail(n - 1, n * acc)  # Tail recursive
```

### 4. Head Recursion
Recursive call is made before other operations.

```python
def print_numbers(n):
    if n > 0:
        print_numbers(n - 1)  # Head recursion
        print(n)  # Print after recursion
```

## Classic Recursive Problems

### 1. Factorial
```python
def factorial(n):
    if n <= 1:
        return 1
    return n * factorial(n - 1)

# Time: O(n), Space: O(n)
```

### 2. Fibonacci Sequence
```python
def fibonacci(n):
    if n <= 1:
        return n
    return fibonacci(n - 1) + fibonacci(n - 2)

# Time: O(2^n), Space: O(n)
# Note: Inefficient! Use DP for better performance
```

### 3. Power Function
```python
def power(x, n):
    if n == 0:
        return 1
    if n < 0:
        return 1 / power(x, -n)
    return x * power(x, n - 1)

# Optimized version (O(log n))
def power_optimized(x, n):
    if n == 0:
        return 1
    if n < 0:
        return 1 / power_optimized(x, -n)
    if n % 2 == 0:
        half = power_optimized(x, n // 2)
        return half * half
    else:
        return x * power_optimized(x, n - 1)
```

### 4. Sum of Array Elements
```python
def array_sum(arr, index=0):
    if index >= len(arr):
        return 0
    return arr[index] + array_sum(arr, index + 1)

# Alternative
def array_sum_slice(arr):
    if not arr:
        return 0
    return arr[0] + array_sum_slice(arr[1:])
```

### 5. Reverse String
```python
def reverse_string(s):
    if len(s) <= 1:
        return s
    return reverse_string(s[1:]) + s[0]
```

### 6. Palindrome Check
```python
def is_palindrome(s):
    if len(s) <= 1:
        return True
    if s[0] != s[-1]:
        return False
    return is_palindrome(s[1:-1])
```

### 7. Binary Search (Recursive)
```python
def binary_search_recursive(arr, target, left, right):
    if left > right:
        return -1
    
    mid = left + (right - left) // 2
    
    if arr[mid] == target:
        return mid
    elif arr[mid] > target:
        return binary_search_recursive(arr, target, left, mid - 1)
    else:
        return binary_search_recursive(arr, target, mid + 1, right)
```

### 8. Tower of Hanoi
```python
def tower_of_hanoi(n, source, destination, auxiliary):
    if n == 1:
        print(f"Move disk 1 from {source} to {destination}")
        return
    
    # Move n-1 disks from source to auxiliary
    tower_of_hanoi(n - 1, source, auxiliary, destination)
    
    # Move largest disk from source to destination
    print(f"Move disk {n} from {source} to {destination}")
    
    # Move n-1 disks from auxiliary to destination
    tower_of_hanoi(n - 1, auxiliary, destination, source)
```

### 9. GCD (Greatest Common Divisor)
```python
def gcd(a, b):
    if b == 0:
        return a
    return gcd(b, a % b)

# Using Euclidean algorithm
```

### 10. Permutations
```python
def permutations(arr):
    if len(arr) <= 1:
        return [arr]
    
    result = []
    for i in range(len(arr)):
        rest = arr[:i] + arr[i+1:]
        for p in permutations(rest):
            result.append([arr[i]] + p)
    
    return result
```

### 11. Subsets
```python
def subsets(arr):
    if not arr:
        return [[]]
    
    rest_subsets = subsets(arr[1:])
    result = []
    
    for subset in rest_subsets:
        result.append(subset)  # Without current element
        result.append([arr[0]] + subset)  # With current element
    
    return result
```

### 12. Tree Traversal (Recursive)
```python
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

def inorder_traversal(root):
    if not root:
        return []
    return inorder_traversal(root.left) + [root.val] + inorder_traversal(root.right)

def preorder_traversal(root):
    if not root:
        return []
    return [root.val] + preorder_traversal(root.left) + preorder_traversal(root.right)

def postorder_traversal(root):
    if not root:
        return []
    return postorder_traversal(root.left) + postorder_traversal(root.right) + [root.val]
```

### 13. Merge Sort (Recursive)
```python
def merge_sort(arr):
    if len(arr) <= 1:
        return arr
    
    mid = len(arr) // 2
    left = merge_sort(arr[:mid])
    right = merge_sort(arr[mid:])
    
    return merge(left, right)

def merge(left, right):
    result = []
    i = j = 0
    
    while i < len(left) and j < len(right):
        if left[i] <= right[j]:
            result.append(left[i])
            i += 1
        else:
            result.append(right[j])
            j += 1
    
    result.extend(left[i:])
    result.extend(right[j:])
    return result
```

### 14. Quick Sort (Recursive)
```python
def quick_sort(arr):
    if len(arr) <= 1:
        return arr
    
    pivot = arr[len(arr) // 2]
    left = [x for x in arr if x < pivot]
    middle = [x for x in arr if x == pivot]
    right = [x for x in arr if x > pivot]
    
    return quick_sort(left) + middle + quick_sort(right)
```

### 15. Depth-First Search (DFS) - Recursive
```python
def dfs_recursive(graph, node, visited):
    visited.add(node)
    print(node)
    
    for neighbor in graph[node]:
        if neighbor not in visited:
            dfs_recursive(graph, neighbor, visited)
```

## Recursion vs Iteration

### When to Use Recursion
- Problem naturally recursive (tree/graph traversal)
- Code is cleaner and more readable
- Divide and conquer problems
- Dynamic programming (with memoization)

### When to Use Iteration
- Simple loops are sufficient
- Stack overflow is a concern
- Performance is critical
- Space efficiency needed

## Converting Recursion to Iteration

### Using Stack
```python
# Recursive DFS
def dfs_recursive(graph, node, visited):
    visited.add(node)
    for neighbor in graph[node]:
        if neighbor not in visited:
            dfs_recursive(graph, neighbor, visited)

# Iterative DFS using stack
def dfs_iterative(graph, start):
    visited = set()
    stack = [start]
    
    while stack:
        node = stack.pop()
        if node not in visited:
            visited.add(node)
            for neighbor in graph[node]:
                stack.append(neighbor)
```

## Common Pitfalls

1. **Missing Base Case**: Causes infinite recursion
2. **Stack Overflow**: Too many recursive calls
3. **Inefficient Recursion**: Not using memoization when needed
4. **Wrong Recursive Case**: Incorrect problem decomposition

## Optimization Techniques

### 1. Memoization (DP)
```python
from functools import lru_cache

@lru_cache(maxsize=None)
def fibonacci_memo(n):
    if n <= 1:
        return n
    return fibonacci_memo(n - 1) + fibonacci_memo(n - 2)
```

### 2. Tail Recursion Optimization
Some languages optimize tail recursion to avoid stack growth.

## Space Complexity Analysis

- **Stack Depth**: Maximum depth of recursion tree
- **Each Call**: Uses O(1) space (unless parameters are large)
- **Total Space**: O(depth) for recursion stack

## Time Complexity Analysis

Analyze using:
- Recursion tree method
- Master theorem (for divide and conquer)
- Substitution method

## Common Problems
- Factorial
- Fibonacci
- Power Function
- String Reversal
- Palindrome Check
- Tree Traversals
- Tower of Hanoi
- Permutations/Combinations
- Subsets
- GCD/LCM
- Merge Sort / Quick Sort
- DFS / BFS (recursive)
- Binary Search


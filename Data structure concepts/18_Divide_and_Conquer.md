# Divide and Conquer

## Definition
Divide and Conquer is an algorithmic paradigm that breaks a problem into smaller subproblems, solves them recursively, and combines their solutions to solve the original problem.

## Key Steps
1. **Divide**: Break problem into smaller subproblems
2. **Conquer**: Solve subproblems recursively
3. **Combine**: Combine solutions of subproblems to solve original problem

## Characteristics
- **Recursive Nature**: Uses recursion to solve subproblems
- **Independent Subproblems**: Subproblems don't overlap (unlike DP)
- **Base Case**: Small enough problems solved directly
- **Efficiency**: Often results in O(n log n) time complexity

## When to Use
- Problem can be broken into similar subproblems
- Subproblems are independent (no overlapping)
- Combining solutions is straightforward
- Natural recursive structure

## Divide and Conquer vs Dynamic Programming
- **Divide and Conquer**: Independent subproblems, no memoization needed
- **Dynamic Programming**: Overlapping subproblems, stores results

## Classic Divide and Conquer Problems

### 1. Binary Search
**Problem**: Search element in sorted array

```python
def binary_search(arr, target, left, right):
    if left > right:
        return -1
    
    mid = left + (right - left) // 2
    
    if arr[mid] == target:
        return mid
    elif arr[mid] > target:
        return binary_search(arr, target, left, mid - 1)  # Conquer left
    else:
        return binary_search(arr, target, mid + 1, right)  # Conquer right
```

**Time Complexity**: O(log n)
**Recurrence**: T(n) = T(n/2) + O(1)

---

### 2. Merge Sort
**Problem**: Sort an array

```python
def merge_sort(arr):
    if len(arr) <= 1:
        return arr  # Base case
    
    # Divide
    mid = len(arr) // 2
    left = merge_sort(arr[:mid])
    right = merge_sort(arr[mid:])
    
    # Conquer & Combine
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

**Time Complexity**: O(n log n)
**Space Complexity**: O(n)
**Recurrence**: T(n) = 2T(n/2) + O(n)

---

### 3. Quick Sort
**Problem**: Sort an array

```python
def quick_sort(arr, low, high):
    if low < high:
        # Divide: Partition around pivot
        pi = partition(arr, low, high)
        
        # Conquer: Sort partitions
        quick_sort(arr, low, pi - 1)
        quick_sort(arr, pi + 1, high)

def partition(arr, low, high):
    pivot = arr[high]
    i = low - 1
    
    for j in range(low, high):
        if arr[j] < pivot:
            i += 1
            arr[i], arr[j] = arr[j], arr[i]
    
    arr[i + 1], arr[high] = arr[high], arr[i + 1]
    return i + 1
```

**Time Complexity**: 
- Average: O(n log n)
- Worst: O(n²)
**Recurrence**: T(n) = T(k) + T(n-k-1) + O(n)

---

### 4. Maximum Subarray Sum (Divide and Conquer)
**Problem**: Find maximum sum of contiguous subarray

```python
def max_subarray_dc(arr, low, high):
    if low == high:
        return arr[low]  # Base case
    
    mid = (low + high) // 2
    
    # Conquer: Find max in left and right halves
    left_max = max_subarray_dc(arr, low, mid)
    right_max = max_subarray_dc(arr, mid + 1, high)
    
    # Combine: Find max crossing middle
    cross_max = max_crossing_sum(arr, low, mid, high)
    
    return max(left_max, right_max, cross_max)

def max_crossing_sum(arr, low, mid, high):
    # Left side
    left_sum = float('-inf')
    total = 0
    for i in range(mid, low - 1, -1):
        total += arr[i]
        left_sum = max(left_sum, total)
    
    # Right side
    right_sum = float('-inf')
    total = 0
    for i in range(mid + 1, high + 1):
        total += arr[i]
        right_sum = max(right_sum, total)
    
    return left_sum + right_sum
```

**Time Complexity**: O(n log n)
**Note**: Kadane's algorithm (DP) is O(n) and preferred

---

### 5. Power Function (x^n)
**Problem**: Calculate x raised to power n efficiently

```python
def power(x, n):
    if n == 0:
        return 1  # Base case
    
    # Divide
    half = power(x, n // 2)
    
    # Conquer & Combine
    if n % 2 == 0:
        return half * half
    else:
        return half * half * x

# Handle negative exponents
def power_negative(x, n):
    if n < 0:
        x = 1 / x
        n = -n
    return power(x, n)
```

**Time Complexity**: O(log n)
**Recurrence**: T(n) = T(n/2) + O(1)

---

### 6. Strassen's Matrix Multiplication
**Problem**: Multiply two matrices efficiently

```python
def strassen_multiply(A, B):
    n = len(A)
    
    if n == 1:
        return [[A[0][0] * B[0][0]]]
    
    # Divide into 4 submatrices
    mid = n // 2
    
    A11 = [row[:mid] for row in A[:mid]]
    A12 = [row[mid:] for row in A[:mid]]
    A21 = [row[:mid] for row in A[mid:]]
    A22 = [row[mid:] for row in A[mid:]]
    
    B11 = [row[:mid] for row in B[:mid]]
    B12 = [row[mid:] for row in B[:mid]]
    B21 = [row[:mid] for row in B[mid:]]
    B22 = [row[mid:] for row in B[mid:]]
    
    # 7 recursive multiplications (instead of 8)
    M1 = strassen_multiply(add_matrix(A11, A22), add_matrix(B11, B22))
    M2 = strassen_multiply(add_matrix(A21, A22), B11)
    M3 = strassen_multiply(A11, subtract_matrix(B12, B22))
    M4 = strassen_multiply(A22, subtract_matrix(B21, B11))
    M5 = strassen_multiply(add_matrix(A11, A12), B22)
    M6 = strassen_multiply(subtract_matrix(A21, A11), add_matrix(B11, B12))
    M7 = strassen_multiply(subtract_matrix(A12, A22), add_matrix(B21, B22))
    
    # Combine results
    C11 = add_matrix(subtract_matrix(add_matrix(M1, M4), M5), M7)
    C12 = add_matrix(M3, M5)
    C21 = add_matrix(M2, M4)
    C22 = add_matrix(subtract_matrix(add_matrix(M1, M3), M2), M6)
    
    # Combine into result matrix
    result = []
    for i in range(mid):
        result.append(C11[i] + C12[i])
    for i in range(mid):
        result.append(C21[i] + C22[i])
    
    return result
```

**Time Complexity**: O(n^log₂7) ≈ O(n^2.81) (better than O(n³) naive)

---

### 7. Closest Pair of Points
**Problem**: Find two closest points in 2D plane

```python
import math

def closest_pair(points):
    points.sort(key=lambda p: p[0])  # Sort by x-coordinate
    return closest_pair_recursive(points)

def closest_pair_recursive(points):
    n = len(points)
    
    if n <= 3:
        return brute_force_closest(points)
    
    # Divide
    mid = n // 2
    mid_point = points[mid]
    
    left_points = points[:mid]
    right_points = points[mid:]
    
    # Conquer
    left_min = closest_pair_recursive(left_points)
    right_min = closest_pair_recursive(right_points)
    
    min_dist = min(left_min, right_min)
    
    # Combine: Check strip around middle
    strip = []
    for point in points:
        if abs(point[0] - mid_point[0]) < min_dist:
            strip.append(point)
    
    strip.sort(key=lambda p: p[1])  # Sort by y-coordinate
    
    return min(min_dist, strip_closest(strip, min_dist))

def strip_closest(strip, min_dist):
    min_val = min_dist
    n = len(strip)
    
    for i in range(n):
        j = i + 1
        while j < n and (strip[j][1] - strip[i][1]) < min_val:
            min_val = min(min_val, distance(strip[i], strip[j]))
            j += 1
    
    return min_val

def distance(p1, p2):
    return math.sqrt((p1[0] - p2[0])**2 + (p1[1] - p2[1])**2)

def brute_force_closest(points):
    min_dist = float('inf')
    n = len(points)
    for i in range(n):
        for j in range(i + 1, n):
            min_dist = min(min_dist, distance(points[i], points[j]))
    return min_dist
```

**Time Complexity**: O(n log² n) or O(n log n) with optimization

---

### 8. Karatsuba Algorithm (Fast Multiplication)
**Problem**: Multiply two large numbers efficiently

```python
def karatsuba_multiply(x, y):
    # Convert to strings for easier manipulation
    x_str, y_str = str(x), str(y)
    
    if len(x_str) == 1 or len(y_str) == 1:
        return x * y  # Base case
    
    n = max(len(x_str), len(y_str))
    m = n // 2
    
    # Split numbers
    a, b = divmod(x, 10**m)
    c, d = divmod(y, 10**m)
    
    # Three recursive multiplications
    z0 = karatsuba_multiply(b, d)
    z1 = karatsuba_multiply((a + b), (c + d))
    z2 = karatsuba_multiply(a, c)
    
    # Combine: ac * 10^(2m) + ((a+b)(c+d) - ac - bd) * 10^m + bd
    return (z2 * 10**(2*m)) + ((z1 - z2 - z0) * 10**m) + z0
```

**Time Complexity**: O(n^log₂3) ≈ O(n^1.585) (better than O(n²))

---

### 9. Count Inversions
**Problem**: Count number of inversions in array (i < j but arr[i] > arr[j])

```python
def count_inversions(arr):
    def merge_count(arr, temp, left, right):
        inv_count = 0
        
        if left < right:
            mid = (left + right) // 2
            
            # Divide & Conquer
            inv_count += merge_count(arr, temp, left, mid)
            inv_count += merge_count(arr, temp, mid + 1, right)
            
            # Combine
            inv_count += merge(arr, temp, left, mid, right)
        
        return inv_count
    
    temp = [0] * len(arr)
    return merge_count(arr, temp, 0, len(arr) - 1)

def merge(arr, temp, left, mid, right):
    inv_count = 0
    i, j, k = left, mid + 1, left
    
    while i <= mid and j <= right:
        if arr[i] <= arr[j]:
            temp[k] = arr[i]
            i += 1
        else:
            temp[k] = arr[j]
            inv_count += (mid - i + 1)  # Count inversions
            j += 1
        k += 1
    
    while i <= mid:
        temp[k] = arr[i]
        i += 1
        k += 1
    
    while j <= right:
        temp[k] = arr[j]
        j += 1
        k += 1
    
    for i in range(left, right + 1):
        arr[i] = temp[i]
    
    return inv_count
```

**Time Complexity**: O(n log n)

---

### 10. Fast Fourier Transform (FFT)
**Problem**: Efficiently compute Discrete Fourier Transform

**Note**: FFT is complex; here's simplified concept:

```python
import cmath

def fft(a):
    n = len(a)
    
    if n == 1:
        return a  # Base case
    
    # Divide: Split into even and odd indices
    even = fft([a[i] for i in range(0, n, 2)])
    odd = fft([a[i] for i in range(1, n, 2)])
    
    # Combine
    result = [0] * n
    w = cmath.exp(-2j * cmath.pi / n)
    w_power = 1
    
    for k in range(n // 2):
        t = w_power * odd[k]
        result[k] = even[k] + t
        result[k + n // 2] = even[k] - t
        w_power *= w
    
    return result
```

**Time Complexity**: O(n log n) (vs O(n²) naive DFT)

---

## Recurrence Relations

Common recurrence relations in Divide and Conquer:

1. **T(n) = T(n/2) + O(1)**
   - Solution: O(log n)
   - Example: Binary Search

2. **T(n) = 2T(n/2) + O(n)**
   - Solution: O(n log n)
   - Example: Merge Sort

3. **T(n) = 2T(n/2) + O(1)**
   - Solution: O(n)
   - Example: Tree traversal

4. **T(n) = T(n-1) + O(1)**
   - Solution: O(n)
   - Example: Linear recursion

## Master Theorem

For recurrence T(n) = aT(n/b) + f(n):

- If f(n) = O(n^log_b(a - ε)): T(n) = Θ(n^log_b(a))
- If f(n) = Θ(n^log_b(a)): T(n) = Θ(n^log_b(a) × log n)
- If f(n) = Ω(n^log_b(a + ε)): T(n) = Θ(f(n))

## Advantages
- Simple and intuitive
- Naturally parallelizable
- Often efficient (O(n log n))
- Clean recursive structure

## Disadvantages
- Recursion overhead
- May not be optimal for all problems
- Memory usage for recursion stack

## Common Problems
- Binary Search
- Merge Sort
- Quick Sort
- Maximum Subarray (Divide & Conquer version)
- Power Function
- Strassen's Matrix Multiplication
- Closest Pair of Points
- Karatsuba Multiplication
- Count Inversions
- Fast Fourier Transform


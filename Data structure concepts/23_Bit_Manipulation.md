# Bit Manipulation

## Definition
Bit manipulation is the act of algorithmically manipulating bits or binary digits. It's one of the most efficient ways to perform operations and is fundamental to low-level programming.

## Why Bit Manipulation?
- **Speed**: Bit operations are extremely fast
- **Space Efficiency**: Can store multiple flags in a single integer
- **Low-Level Control**: Direct hardware-level operations
- **Competitive Programming**: Essential for optimization

## Basic Bit Operations

### 1. AND (&)
Returns 1 if both bits are 1, else 0.

```python
# 5 & 3 = 1
#  101 (5)
# &011 (3)
# -----
#  001 (1)

result = 5 & 3  # Returns 1
```

### 2. OR (|)
Returns 1 if at least one bit is 1, else 0.

```python
# 5 | 3 = 7
#  101 (5)
# |011 (3)
# -----
#  111 (7)

result = 5 | 3  # Returns 7
```

### 3. XOR (^)
Returns 1 if bits are different, else 0.

```python
# 5 ^ 3 = 6
#  101 (5)
# ^011 (3)
# -----
#  110 (6)

result = 5 ^ 3  # Returns 6
```

### 4. NOT (~)
Flips all bits (one's complement).

```python
# ~5 = -6 (in two's complement)
result = ~5  # Returns -6
```

### 5. Left Shift (<<)
Shifts bits to the left, fills with 0s.

```python
# 5 << 2 = 20
# 101 << 2 = 10100 = 20
# Equivalent to: 5 * (2^2) = 5 * 4 = 20

result = 5 << 2  # Returns 20
```

### 6. Right Shift (>>)
Shifts bits to the right.

```python
# 20 >> 2 = 5
# 10100 >> 2 = 101 = 5
# Equivalent to: 20 // (2^2) = 20 // 4 = 5

result = 20 >> 2  # Returns 5
```

## Common Bit Tricks

### 1. Check if Number is Even/Odd
```python
def is_even(n):
    return (n & 1) == 0

def is_odd(n):
    return (n & 1) == 1
```

### 2. Check if Power of 2
```python
def is_power_of_2(n):
    return n > 0 and (n & (n - 1)) == 0
```

### 3. Get Rightmost Set Bit
```python
def get_rightmost_set_bit(n):
    return n & (-n)

# Example: 12 = 1100
# -12 in two's complement = ...11110100
# 12 & (-12) = 100 = 4
```

### 4. Clear Rightmost Set Bit
```python
def clear_rightmost_set_bit(n):
    return n & (n - 1)
```

### 5. Count Set Bits (Hamming Weight)
```python
def count_set_bits(n):
    count = 0
    while n:
        count += 1
        n &= (n - 1)  # Clear rightmost set bit
    return count

# Alternative using built-in
def count_set_bits_builtin(n):
    return bin(n).count('1')
```

### 6. Set ith Bit
```python
def set_bit(n, i):
    return n | (1 << i)
```

### 7. Clear ith Bit
```python
def clear_bit(n, i):
    return n & ~(1 << i)
```

### 8. Toggle ith Bit
```python
def toggle_bit(n, i):
    return n ^ (1 << i)
```

### 9. Get ith Bit
```python
def get_bit(n, i):
    return (n >> i) & 1
```

### 10. Check if ith Bit is Set
```python
def is_bit_set(n, i):
    return (n >> i) & 1 == 1
    # or: return (n & (1 << i)) != 0
```

## Classic Bit Manipulation Problems

### 1. Single Number
**Problem**: Find the only element that appears once (others appear twice)

```python
def single_number(nums):
    result = 0
    for num in nums:
        result ^= num  # XOR cancels duplicates
    return result

# Time: O(n), Space: O(1)
```

### 2. Single Number II
**Problem**: Find the only element that appears once (others appear 3 times)

```python
def single_number_ii(nums):
    ones = twos = 0
    for num in nums:
        ones = (ones ^ num) & ~twos
        twos = (twos ^ num) & ~ones
    return ones
```

### 3. Missing Number
**Problem**: Find missing number in array [0, n]

```python
def missing_number(nums):
    n = len(nums)
    result = n
    for i, num in enumerate(nums):
        result ^= i ^ num
    return result
```

### 4. Reverse Bits
**Problem**: Reverse bits of a 32-bit unsigned integer

```python
def reverse_bits(n):
    result = 0
    for i in range(32):
        result <<= 1
        result |= (n & 1)
        n >>= 1
    return result
```

### 5. Number of 1 Bits
**Problem**: Count number of set bits

```python
def hamming_weight(n):
    count = 0
    while n:
        count += 1
        n &= (n - 1)
    return count
```

### 6. Power of Two
**Problem**: Check if number is power of 2

```python
def is_power_of_two(n):
    return n > 0 and (n & (n - 1)) == 0
```

### 7. Sum of Two Integers (Without +)
**Problem**: Add two integers without using + or - operators

```python
def get_sum(a, b):
    while b != 0:
        carry = a & b  # Carry
        a = a ^ b      # Sum without carry
        b = carry << 1 # Shift carry
    return a
```

### 8. Bitwise AND of Range
**Problem**: Find bitwise AND of numbers in range [m, n]

```python
def range_bitwise_and(m, n):
    shift = 0
    while m < n:
        m >>= 1
        n >>= 1
        shift += 1
    return m << shift
```

### 9. Counting Bits
**Problem**: Count set bits for all numbers from 0 to n

```python
def count_bits(n):
    result = [0] * (n + 1)
    for i in range(1, n + 1):
        result[i] = result[i & (i - 1)] + 1
    return result

# Alternative
def count_bits_alternative(n):
    result = [0] * (n + 1)
    for i in range(1, n + 1):
        result[i] = result[i >> 1] + (i & 1)
    return result
```

### 10. Maximum XOR of Two Numbers
**Problem**: Find maximum XOR value

```python
class TrieNode:
    def __init__(self):
        self.children = {}

def find_maximum_xor(nums):
    # Build trie
    root = TrieNode()
    for num in nums:
        node = root
        for i in range(31, -1, -1):
            bit = (num >> i) & 1
            if bit not in node.children:
                node.children[bit] = TrieNode()
            node = node.children[bit]
    
    # Find max XOR
    max_xor = 0
    for num in nums:
        node = root
        xor_val = 0
        for i in range(31, -1, -1):
            bit = (num >> i) & 1
            toggle_bit = 1 - bit  # Opposite bit
            
            if toggle_bit in node.children:
                xor_val |= (1 << i)
                node = node.children[toggle_bit]
            else:
                node = node.children[bit]
        
        max_xor = max(max_xor, xor_val)
    
    return max_xor
```

## Bit Manipulation in Data Structures

### 1. Bitmask for Subsets
```python
def generate_subsets(nums):
    n = len(nums)
    subsets = []
    
    # Each bitmask represents a subset
    for mask in range(1 << n):
        subset = []
        for i in range(n):
            if mask & (1 << i):
                subset.append(nums[i])
        subsets.append(subset)
    
    return subsets
```

### 2. Union-Find with Bitmask
```python
# Using bits to track sets
def find_parent(parent, i):
    if parent[i] != i:
        parent[i] = find_parent(parent, parent[i])
    return parent[i]
```

### 3. Graph Representation (Adjacency Matrix as Bits)
```python
# Each row in adjacency matrix can be represented as bitset
class Graph:
    def __init__(self, n):
        self.n = n
        self.adj = [0] * n  # Each element is a bitmask
    
    def add_edge(self, u, v):
        self.adj[u] |= (1 << v)
        self.adj[v] |= (1 << u)
    
    def has_edge(self, u, v):
        return (self.adj[u] >> v) & 1 == 1
```

## Useful Bit Patterns

### Common Patterns
- `x & (-x)`: Rightmost set bit
- `x & (x - 1)`: Clear rightmost set bit
- `x | (x + 1)`: Set rightmost unset bit
- `x & (x + 1)`: Clear trailing ones
- `x | (x - 1)`: Set trailing zeros

### Bit Manipulation for Optimization

#### Fast Multiplication/Division by Powers of 2
```python
# Multiply by 2^k
result = n << k

# Divide by 2^k
result = n >> k

# Modulo by 2^k
result = n & ((1 << k) - 1)
```

## Common Problems
- Single Number
- Single Number II/III
- Missing Number
- Reverse Bits
- Number of 1 Bits
- Power of Two
- Sum of Two Integers
- Bitwise AND of Range
- Counting Bits
- Maximum XOR of Two Numbers
- Subsets (using bitmask)
- Gray Code
- UTF-8 Validation


# Dynamic Programming

## Definition
Dynamic Programming (DP) is a method for solving complex problems by breaking them down into simpler subproblems. It stores the results of subproblems to avoid recomputing them, thus optimizing the solution.

## Key Characteristics
- **Optimal Substructure**: Optimal solution contains optimal solutions to subproblems
- **Overlapping Subproblems**: Same subproblems are solved multiple times
- **Memoization/Tabulation**: Store results to avoid recomputation

## Two Approaches

### 1. Top-Down (Memoization)
- Recursive approach
- Store results as you compute them
- Also called "Memoization"
- More intuitive, uses recursion stack

### 2. Bottom-Up (Tabulation)
- Iterative approach
- Build solution from smallest subproblems
- Also called "Tabulation"
- More efficient (no recursion overhead)

## DP vs Other Approaches

### DP vs Divide and Conquer
- **DP**: Overlapping subproblems, stores results
- **Divide and Conquer**: Independent subproblems, no storage

### DP vs Greedy
- **DP**: Explores all possibilities, guarantees optimal
- **Greedy**: Makes locally optimal choice, may not be globally optimal

## When to Use DP
1. **Optimization Problems**: Finding min/max/count
2. **Decision Problems**: Yes/No questions
3. **Counting Problems**: Number of ways to do something
4. **Overlapping Subproblems**: Same calculations repeated
5. **Optimal Substructure**: Solution depends on optimal subproblems

## Steps to Solve DP Problems

1. **Identify the Problem**: Recognize DP characteristics
2. **Define State**: What information do we need to track?
3. **Formulate Recurrence Relation**: How do states relate?
4. **Base Cases**: Smallest subproblems that can be solved directly
5. **Choose Approach**: Top-down or bottom-up
6. **Optimize Space**: If possible, reduce space complexity

## Classic DP Problems

### 1. Fibonacci Sequence
**Problem**: Find nth Fibonacci number
**Recurrence**: F(n) = F(n-1) + F(n-2)

```python
# Naive Recursive (O(2^n))
def fib_naive(n):
    if n <= 1:
        return n
    return fib_naive(n-1) + fib_naive(n-2)

# Top-Down DP (Memoization) - O(n) time, O(n) space
def fib_memo(n, memo={}):
    if n in memo:
        return memo[n]
    if n <= 1:
        return n
    memo[n] = fib_memo(n-1, memo) + fib_memo(n-2, memo)
    return memo[n]

# Bottom-Up DP (Tabulation) - O(n) time, O(n) space
def fib_tabulation(n):
    if n <= 1:
        return n
    dp = [0] * (n + 1)
    dp[1] = 1
    for i in range(2, n + 1):
        dp[i] = dp[i-1] + dp[i-2]
    return dp[n]

# Space-Optimized - O(n) time, O(1) space
def fib_optimized(n):
    if n <= 1:
        return n
    prev2, prev1 = 0, 1
    for i in range(2, n + 1):
        curr = prev1 + prev2
        prev2, prev1 = prev1, curr
    return prev1
```

---

### 2. Climbing Stairs
**Problem**: Count ways to reach nth step (can climb 1 or 2 steps at a time)

```python
def climb_stairs(n):
    if n <= 2:
        return n
    dp = [0] * (n + 1)
    dp[1] = 1
    dp[2] = 2
    for i in range(3, n + 1):
        dp[i] = dp[i-1] + dp[i-2]
    return dp[n]

# Space-optimized
def climb_stairs_optimized(n):
    if n <= 2:
        return n
    prev2, prev1 = 1, 2
    for i in range(3, n + 1):
        curr = prev1 + prev2
        prev2, prev1 = prev1, curr
    return prev1
```

---

### 3. 0/1 Knapsack Problem
**Problem**: Maximize value with weight constraint (each item can be taken at most once)

```python
def knapsack(weights, values, capacity):
    n = len(weights)
    dp = [[0 for _ in range(capacity + 1)] for _ in range(n + 1)]
    
    for i in range(1, n + 1):
        for w in range(1, capacity + 1):
            if weights[i-1] <= w:
                dp[i][w] = max(
                    dp[i-1][w],  # Don't take
                    dp[i-1][w - weights[i-1]] + values[i-1]  # Take
                )
            else:
                dp[i][w] = dp[i-1][w]
    
    return dp[n][capacity]

# Space-optimized (1D array)
def knapsack_optimized(weights, values, capacity):
    dp = [0] * (capacity + 1)
    
    for i in range(len(weights)):
        for w in range(capacity, weights[i] - 1, -1):
            dp[w] = max(dp[w], dp[w - weights[i]] + values[i])
    
    return dp[capacity]
```

**Time Complexity**: O(n × capacity)
**Space Complexity**: O(capacity) with optimization

---

### 4. Longest Common Subsequence (LCS)
**Problem**: Find length of longest common subsequence between two strings

```python
def lcs(text1, text2):
    m, n = len(text1), len(text2)
    dp = [[0 for _ in range(n + 1)] for _ in range(m + 1)]
    
    for i in range(1, m + 1):
        for j in range(1, n + 1):
            if text1[i-1] == text2[j-1]:
                dp[i][j] = dp[i-1][j-1] + 1
            else:
                dp[i][j] = max(dp[i-1][j], dp[i][j-1])
    
    return dp[m][n]

# Reconstruct LCS string
def lcs_string(text1, text2):
    m, n = len(text1), len(text2)
    dp = [[0 for _ in range(n + 1)] for _ in range(m + 1)]
    
    # Build DP table
    for i in range(1, m + 1):
        for j in range(1, n + 1):
            if text1[i-1] == text2[j-1]:
                dp[i][j] = dp[i-1][j-1] + 1
            else:
                dp[i][j] = max(dp[i-1][j], dp[i][j-1])
    
    # Reconstruct
    result = []
    i, j = m, n
    while i > 0 and j > 0:
        if text1[i-1] == text2[j-1]:
            result.append(text1[i-1])
            i -= 1
            j -= 1
        elif dp[i-1][j] > dp[i][j-1]:
            i -= 1
        else:
            j -= 1
    
    return ''.join(reversed(result))
```

---

### 5. Longest Increasing Subsequence (LIS)
**Problem**: Find length of longest strictly increasing subsequence

```python
# O(n²) approach
def lis(nums):
    n = len(nums)
    dp = [1] * n
    
    for i in range(1, n):
        for j in range(i):
            if nums[j] < nums[i]:
                dp[i] = max(dp[i], dp[j] + 1)
    
    return max(dp)

# O(n log n) approach using binary search
def lis_optimized(nums):
    if not nums:
        return 0
    
    tail = [nums[0]]
    
    for num in nums[1:]:
        if num > tail[-1]:
            tail.append(num)
        else:
            # Binary search for the smallest element >= num
            left, right = 0, len(tail) - 1
            while left < right:
                mid = (left + right) // 2
                if tail[mid] < num:
                    left = mid + 1
                else:
                    right = mid
            tail[left] = num
    
    return len(tail)
```

---

### 6. Edit Distance (Levenshtein Distance)
**Problem**: Minimum operations to convert string1 to string2 (insert, delete, replace)

```python
def edit_distance(word1, word2):
    m, n = len(word1), len(word2)
    dp = [[0 for _ in range(n + 1)] for _ in range(m + 1)]
    
    # Base cases
    for i in range(m + 1):
        dp[i][0] = i
    for j in range(n + 1):
        dp[0][j] = j
    
    for i in range(1, m + 1):
        for j in range(1, n + 1):
            if word1[i-1] == word2[j-1]:
                dp[i][j] = dp[i-1][j-1]  # No operation
            else:
                dp[i][j] = 1 + min(
                    dp[i-1][j],      # Delete
                    dp[i][j-1],      # Insert
                    dp[i-1][j-1]     # Replace
                )
    
    return dp[m][n]
```

---

### 7. Coin Change
**Problem**: Minimum coins needed to make amount (unlimited coins)

```python
def coin_change(coins, amount):
    dp = [float('inf')] * (amount + 1)
    dp[0] = 0
    
    for coin in coins:
        for i in range(coin, amount + 1):
            dp[i] = min(dp[i], dp[i - coin] + 1)
    
    return dp[amount] if dp[amount] != float('inf') else -1

# Count ways to make amount
def coin_change_ways(coins, amount):
    dp = [0] * (amount + 1)
    dp[0] = 1
    
    for coin in coins:
        for i in range(coin, amount + 1):
            dp[i] += dp[i - coin]
    
    return dp[amount]
```

---

### 8. Maximum Subarray Sum (Kadane's Algorithm)
**Problem**: Find maximum sum of contiguous subarray

```python
def max_subarray(nums):
    max_sum = current_sum = nums[0]
    
    for num in nums[1:]:
        current_sum = max(num, current_sum + num)
        max_sum = max(max_sum, current_sum)
    
    return max_sum

# With indices
def max_subarray_with_indices(nums):
    max_sum = current_sum = nums[0]
    start = end = 0
    temp_start = 0
    
    for i in range(1, len(nums)):
        if current_sum < 0:
            current_sum = nums[i]
            temp_start = i
        else:
            current_sum += nums[i]
        
        if current_sum > max_sum:
            max_sum = current_sum
            start = temp_start
            end = i
    
    return max_sum, start, end
```

---

### 9. Unique Paths
**Problem**: Count unique paths from top-left to bottom-right (can move right or down)

```python
def unique_paths(m, n):
    dp = [[1 for _ in range(n)] for _ in range(m)]
    
    for i in range(1, m):
        for j in range(1, n):
            dp[i][j] = dp[i-1][j] + dp[i][j-1]
    
    return dp[m-1][n-1]

# Space-optimized
def unique_paths_optimized(m, n):
    dp = [1] * n
    
    for i in range(1, m):
        for j in range(1, n):
            dp[j] += dp[j-1]
    
    return dp[n-1]
```

---

## DP Patterns

### Pattern 1: 1D DP
- Single array for state
- Examples: Fibonacci, Climbing Stairs

### Pattern 2: 2D DP
- 2D array for state
- Examples: LCS, Edit Distance, Unique Paths

### Pattern 3: String DP
- DP on strings
- Examples: Longest Palindromic Substring, Edit Distance

### Pattern 4: Interval DP
- DP on intervals
- Examples: Matrix Chain Multiplication, Palindrome Partitioning

### Pattern 5: Tree DP
- DP on trees
- Examples: Maximum Path Sum in Binary Tree

## Common Problems by Category

### 1D DP
- Fibonacci
- Climbing Stairs
- House Robber
- Decode Ways
- Coin Change

### 2D DP
- Unique Paths
- Longest Common Subsequence
- Edit Distance
- Interleaving String
- Minimum Path Sum

### String DP
- Longest Palindromic Substring
- Longest Palindromic Subsequence
- Regular Expression Matching
- Wildcard Matching

### Knapsack Variants
- 0/1 Knapsack
- Unbounded Knapsack
- Subset Sum
- Partition Equal Subset Sum

## Tips for Solving DP Problems
1. Start with recursive solution
2. Identify overlapping subproblems
3. Add memoization
4. Convert to iterative (tabulation) if needed
5. Optimize space if possible
6. Draw state transition diagrams
7. Practice pattern recognition

## Time and Space Complexity Optimization
- **Time**: Usually polynomial (O(n²), O(n³))
- **Space**: Can often optimize from O(n²) to O(n) or O(1)
- **Techniques**: 
  - Use 1D array instead of 2D
  - Reuse same array
  - Only store necessary states


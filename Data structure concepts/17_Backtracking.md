# Backtracking

## Definition
Backtracking is an algorithmic technique for solving problems recursively by trying to build a solution incrementally, removing solutions that fail to satisfy the constraints of the problem at any point in time.

## Characteristics
- **Systematic Search**: Explores all possible solutions
- **Incremental Construction**: Builds solution step by step
- **Constraint Checking**: Abandons partial solutions that violate constraints
- **Pruning**: Stops exploring paths that won't lead to solution
- **Recursive Nature**: Uses recursion to explore solution space

## How Backtracking Works

```
1. Choose: Make a choice
2. Explore: Recursively solve with that choice
3. Unchoose: Backtrack (undo the choice)
4. Try next option or return
```

## Template for Backtracking

```python
def backtrack(candidate):
    if is_solution(candidate):
        add_to_solutions(candidate)
        return
    
    for next_candidate in generate_candidates(candidate):
        if is_valid(next_candidate):
            make_move(next_candidate)
            backtrack(next_candidate)
            undo_move(next_candidate)  # Backtrack
```

## Classic Backtracking Problems

### 1. N-Queens Problem
**Problem**: Place N queens on N×N chessboard so no two queens attack each other

```python
def solve_n_queens(n):
    board = [['.' for _ in range(n)] for _ in range(n)]
    solutions = []
    
    def is_safe(row, col):
        # Check column
        for i in range(row):
            if board[i][col] == 'Q':
                return False
        
        # Check diagonal (top-left to bottom-right)
        i, j = row - 1, col - 1
        while i >= 0 and j >= 0:
            if board[i][j] == 'Q':
                return False
            i -= 1
            j -= 1
        
        # Check diagonal (top-right to bottom-left)
        i, j = row - 1, col + 1
        while i >= 0 and j < n:
            if board[i][j] == 'Q':
                return False
            i -= 1
            j += 1
        
        return True
    
    def backtrack(row):
        if row == n:
            solutions.append([''.join(row) for row in board])
            return
        
        for col in range(n):
            if is_safe(row, col):
                board[row][col] = 'Q'  # Choose
                backtrack(row + 1)      # Explore
                board[row][col] = '.'   # Unchoose (Backtrack)
    
    backtrack(0)
    return solutions
```

**Time Complexity**: O(N!) - worst case
**Space Complexity**: O(N²) for board, O(N) for recursion stack

---

### 2. Sudoku Solver
**Problem**: Solve a 9×9 Sudoku puzzle

```python
def solve_sudoku(board):
    def is_valid(row, col, num):
        # Check row
        for j in range(9):
            if board[row][j] == str(num):
                return False
        
        # Check column
        for i in range(9):
            if board[i][col] == str(num):
                return False
        
        # Check 3x3 box
        box_row, box_col = (row // 3) * 3, (col // 3) * 3
        for i in range(box_row, box_row + 3):
            for j in range(box_col, box_col + 3):
                if board[i][j] == str(num):
                    return False
        
        return True
    
    def backtrack():
        for i in range(9):
            for j in range(9):
                if board[i][j] == '.':
                    for num in range(1, 10):
                        if is_valid(i, j, num):
                            board[i][j] = str(num)  # Choose
                            if backtrack():          # Explore
                                return True
                            board[i][j] = '.'        # Unchoose (Backtrack)
                    return False  # No valid number found
        return True  # All cells filled
    
    backtrack()
    return board
```

---

### 3. Generate Parentheses
**Problem**: Generate all valid combinations of n pairs of parentheses

```python
def generate_parentheses(n):
    result = []
    
    def backtrack(current, open_count, close_count):
        # Base case: valid combination
        if len(current) == 2 * n:
            result.append(current)
            return
        
        # Add open parenthesis if we have remaining
        if open_count < n:
            backtrack(current + '(', open_count + 1, close_count)
        
        # Add close parenthesis if valid
        if close_count < open_count:
            backtrack(current + ')', open_count, close_count + 1)
    
    backtrack('', 0, 0)
    return result
```

**Time Complexity**: O(4^n / √n) - Catalan number
**Space Complexity**: O(n) for recursion stack

---

### 4. Combination Sum
**Problem**: Find all unique combinations where numbers sum to target

```python
def combination_sum(candidates, target):
    result = []
    
    def backtrack(remaining, combination, start):
        if remaining == 0:
            result.append(combination[:])  # Copy
            return
        
        if remaining < 0:
            return
        
        for i in range(start, len(candidates)):
            combination.append(candidates[i])           # Choose
            backtrack(remaining - candidates[i], 
                     combination, i)                    # Explore (i, not i+1 to allow reuse)
            combination.pop()                           # Unchoose (Backtrack)
    
    backtrack(target, [], 0)
    return result

# Without reusing elements (each number used once)
def combination_sum2(candidates, target):
    candidates.sort()
    result = []
    
    def backtrack(remaining, combination, start):
        if remaining == 0:
            result.append(combination[:])
            return
        
        if remaining < 0:
            return
        
        for i in range(start, len(candidates)):
            # Skip duplicates
            if i > start and candidates[i] == candidates[i-1]:
                continue
            
            combination.append(candidates[i])
            backtrack(remaining - candidates[i], combination, i + 1)
            combination.pop()
    
    backtrack(target, [], 0)
    return result
```

---

### 5. Permutations
**Problem**: Generate all permutations of array

```python
def permute(nums):
    result = []
    
    def backtrack(current):
        if len(current) == len(nums):
            result.append(current[:])
            return
        
        for num in nums:
            if num not in current:  # Constraint: no duplicates
                current.append(num)     # Choose
                backtrack(current)      # Explore
                current.pop()           # Unchoose (Backtrack)
    
    backtrack([])
    return result

# More efficient (swapping)
def permute_swap(nums):
    result = []
    
    def backtrack(start):
        if start == len(nums):
            result.append(nums[:])
            return
        
        for i in range(start, len(nums)):
            nums[start], nums[i] = nums[i], nums[start]  # Choose
            backtrack(start + 1)                          # Explore
            nums[start], nums[i] = nums[i], nums[start]  # Unchoose (Backtrack)
    
    backtrack(0)
    return result
```

---

### 6. Subsets / Power Set
**Problem**: Generate all subsets of array

```python
def subsets(nums):
    result = []
    
    def backtrack(start, current):
        result.append(current[:])  # Add current subset
        
        for i in range(start, len(nums)):
            current.append(nums[i])    # Choose
            backtrack(i + 1, current)  # Explore
            current.pop()              # Unchoose (Backtrack)
    
    backtrack(0, [])
    return result
```

**Time Complexity**: O(2^n × n)
**Space Complexity**: O(n) for recursion stack

---

### 7. Word Search
**Problem**: Find if word exists in 2D grid

```python
def exist(board, word):
    rows, cols = len(board), len(board[0])
    
    def backtrack(row, col, index):
        # Base case: word found
        if index == len(word):
            return True
        
        # Boundary and constraint checks
        if (row < 0 or row >= rows or 
            col < 0 or col >= cols or 
            board[row][col] != word[index]):
            return False
        
        # Mark as visited
        temp = board[row][col]
        board[row][col] = '#'
        
        # Explore all directions
        found = (backtrack(row + 1, col, index + 1) or
                backtrack(row - 1, col, index + 1) or
                backtrack(row, col + 1, index + 1) or
                backtrack(row, col - 1, index + 1))
        
        # Backtrack: restore original value
        board[row][col] = temp
        
        return found
    
    # Try starting from each cell
    for i in range(rows):
        for j in range(cols):
            if backtrack(i, j, 0):
                return True
    
    return False
```

---

### 8. Letter Combinations of Phone Number
**Problem**: Generate all letter combinations from phone number

```python
def letter_combinations(digits):
    if not digits:
        return []
    
    phone_map = {
        '2': 'abc', '3': 'def', '4': 'ghi', '5': 'jkl',
        '6': 'mno', '7': 'pqrs', '8': 'tuv', '9': 'wxyz'
    }
    
    result = []
    
    def backtrack(index, current):
        if index == len(digits):
            result.append(current)
            return
        
        digit = digits[index]
        for letter in phone_map[digit]:
            backtrack(index + 1, current + letter)
    
    backtrack(0, '')
    return result
```

---

### 9. Palindrome Partitioning
**Problem**: Partition string so every substring is palindrome

```python
def partition(s):
    result = []
    
    def is_palindrome(sub):
        return sub == sub[::-1]
    
    def backtrack(start, current):
        if start == len(s):
            result.append(current[:])
            return
        
        for end in range(start + 1, len(s) + 1):
            substring = s[start:end]
            if is_palindrome(substring):
                current.append(substring)          # Choose
                backtrack(end, current)            # Explore
                current.pop()                      # Unchoose (Backtrack)
    
    backtrack(0, [])
    return result
```

---

### 10. Rat in a Maze
**Problem**: Find path from source to destination in maze

```python
def solve_maze(maze, start, end):
    rows, cols = len(maze), len(maze[0])
    path = []
    visited = [[False for _ in range(cols)] for _ in range(rows)]
    
    def is_valid(row, col):
        return (0 <= row < rows and 0 <= col < cols and
                maze[row][col] == 1 and not visited[row][col])
    
    def backtrack(row, col):
        # Base case: reached destination
        if (row, col) == end:
            path.append((row, col))
            return True
        
        if not is_valid(row, col):
            return False
        
        visited[row][col] = True  # Mark as visited
        path.append((row, col))   # Add to path
        
        # Try all 4 directions
        directions = [(0, 1), (1, 0), (0, -1), (-1, 0)]
        for dr, dc in directions:
            if backtrack(row + dr, col + dc):
                return True
        
        # Backtrack
        path.pop()
        visited[row][col] = False
        return False
    
    if backtrack(start[0], start[1]):
        return path
    return []
```

---

## Backtracking vs Other Techniques

### Backtracking vs Dynamic Programming
- **Backtracking**: Explores all possibilities, may repeat subproblems
- **DP**: Stores results, no repetition

### Backtracking vs DFS
- **Backtracking**: Prunes invalid paths, builds solution incrementally
- **DFS**: Explores entire graph, doesn't build solution

### Backtracking vs Brute Force
- **Backtracking**: Prunes invalid paths early
- **Brute Force**: Explores all possibilities without pruning

## Optimization Techniques

1. **Early Pruning**: Check constraints early
2. **Constraint Propagation**: Use constraints to reduce search space
3. **Memoization**: Cache results for repeated subproblems (DP + Backtracking)
4. **Heuristics**: Use heuristics to choose better branches first

## Common Patterns

1. **Make choice → Recurse → Undo choice**
2. **Build solution incrementally**
3. **Check constraints before exploring**
4. **Return early when solution found** (if only one needed)

## Common Problems
- N-Queens
- Sudoku Solver
- Generate Parentheses
- Combination Sum
- Permutations
- Subsets
- Word Search
- Letter Combinations
- Palindrome Partitioning
- Rat in a Maze
- Word Break II
- Restore IP Addresses


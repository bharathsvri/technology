# Sets

## Definition
A set is a collection of unique elements with no particular order (unordered set) or sorted order (sorted set). Sets are used to store distinct items and support membership testing, set operations, and elimination of duplicates.

## Characteristics
- **Unique Elements**: No duplicate values allowed
- **Unordered (Hash Set)**: No guaranteed order
- **Ordered (Tree Set)**: Elements in sorted order
- **Fast Membership Testing**: O(1) average case for hash sets
- **Set Operations**: Union, intersection, difference

## Types of Sets

### 1. Hash Set (Unordered Set)
- Implemented using hash table
- O(1) average case operations
- No order guaranteed
- Faster operations

### 2. Tree Set (Sorted Set)
- Implemented using balanced BST (Red-Black tree)
- O(log n) operations
- Maintains sorted order
- Supports range queries

## Operations

### Core Operations
1. **Add/Insert**: Add element to set
2. **Remove/Delete**: Remove element from set
3. **Contains/Lookup**: Check if element exists
4. **Size**: Get number of elements
5. **IsEmpty**: Check if set is empty
6. **Clear**: Remove all elements

### Set Operations
1. **Union (A ∪ B)**: All elements in A or B
2. **Intersection (A ∩ B)**: Elements in both A and B
3. **Difference (A - B)**: Elements in A but not in B
4. **Symmetric Difference (A △ B)**: Elements in A or B but not both
5. **Subset**: Check if A is subset of B
6. **Superset**: Check if A contains B

## Time Complexity

### Hash Set Operations
| Operation | Average Case | Worst Case |
|-----------|--------------|------------|
| Insert | O(1) | O(n) |
| Delete | O(1) | O(n) |
| Contains | O(1) | O(n) |
| Union | O(m + n) | O(m + n) |
| Intersection | O(min(m, n)) | O(m + n) |

### Tree Set Operations
| Operation | Time Complexity |
|-----------|----------------|
| Insert | O(log n) |
| Delete | O(log n) |
| Contains | O(log n) |
| Min/Max | O(log n) |
| Range Query | O(log n + k) |

**Note**: m and n are sizes of sets involved in operations

## Space Complexity
- O(n) where n is the number of unique elements

## Advantages
- Fast membership testing
- Automatic duplicate elimination
- Efficient set operations
- Simple API
- Memory efficient (only stores unique elements)

## Disadvantages
- No order in hash sets (unless sorted set used)
- No indexing (can't access by position)
- No duplicate values (by design, but sometimes needed)

## Use Cases
- Removing duplicates from collection
- Membership testing (fast lookup)
- Set operations (union, intersection)
- Tracking visited nodes in graph algorithms
- Maintaining unique collection
- Tag systems
- Permission checking
- Distinct value counting

## Example Applications

### Duplicate Removal
```python
# Remove duplicates from list
numbers = [1, 2, 2, 3, 3, 3, 4]
unique = set(numbers)
# Result: {1, 2, 3, 4}
```

### Membership Testing
```python
# Fast lookup
visited = set()
if node not in visited:  # O(1) average
    visited.add(node)
    process(node)
```

### Set Operations
```python
A = {1, 2, 3, 4}
B = {3, 4, 5, 6}

union = A | B          # {1, 2, 3, 4, 5, 6}
intersection = A & B   # {3, 4}
difference = A - B     # {1, 2}
symmetric = A ^ B      # {1, 2, 5, 6}
```

### Graph Algorithms
```python
# Track visited nodes
visited = set()
def dfs(node):
    if node in visited:  # O(1) check
        return
    visited.add(node)
    for neighbor in graph[node]:
        dfs(neighbor)
```

## Implementation Details

### Hash Set Implementation
- Uses hash table internally
- Hash function maps element to bucket
- Handles collisions using chaining or open addressing
- Load factor affects performance

### Tree Set Implementation
- Uses balanced BST (typically Red-Black tree)
- Maintains sorted order
- Supports ordered traversal
- More memory overhead than hash set

## Common Problems
- Contains Duplicate
- Intersection of Two Arrays
- Longest Consecutive Sequence
- Word Break (using set for dictionary)
- Group Anagrams
- Valid Sudoku
- First Missing Positive
- Find All Duplicates

## When to Use Hash Set vs Tree Set

### Use Hash Set When:
- Order doesn't matter
- Need fastest average-case performance
- Just need membership testing
- Don't need range queries

### Use Tree Set When:
- Need sorted order
- Need range queries
- Need to find min/max efficiently
- Want predictable O(log n) performance

## Best Practices
- Use sets for membership testing instead of lists
- Use sets to remove duplicates
- Choose hash set for speed, tree set for ordering
- Be aware of hashable requirement (for hash sets)
- Consider memory overhead vs performance
- Use set operations for efficient algorithms

## Language-Specific Notes

### Python
- `set`: Hash set implementation
- `frozenset`: Immutable set
- Set comprehensions supported

### Java
- `HashSet`: Hash set implementation
- `TreeSet`: Sorted set implementation
- `LinkedHashSet`: Maintains insertion order

### C++
- `unordered_set`: Hash set
- `set`: Tree set (sorted)


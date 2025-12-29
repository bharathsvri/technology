# Maps / Dictionaries

## Definition
A map (also called dictionary, associative array, or hash map) is a data structure that stores key-value pairs. Each key maps to exactly one value, and keys must be unique. Maps provide efficient lookup, insertion, and deletion operations.

## Characteristics
- **Key-Value Pairs**: Stores data as (key, value) mappings
- **Unique Keys**: Each key appears only once
- **Fast Lookup**: O(1) average case for hash maps
- **Dynamic Size**: Can grow/shrink as needed
- **Flexible Keys**: Can use various data types as keys

## Types of Maps

### 1. Hash Map (Unordered Map)
- Implemented using hash table
- O(1) average case operations
- No order guaranteed
- Faster operations

### 2. Tree Map (Ordered Map)
- Implemented using balanced BST (Red-Black tree)
- O(log n) operations
- Maintains sorted order by keys
- Supports range queries

### 3. Linked Hash Map
- Maintains insertion order
- Combines hash table and linked list
- O(1) operations with ordering

## Operations

### Core Operations
1. **Put/Insert**: Add or update key-value pair
2. **Get/Retrieve**: Get value by key
3. **Remove/Delete**: Remove key-value pair
4. **ContainsKey**: Check if key exists
5. **ContainsValue**: Check if value exists
6. **Size**: Get number of key-value pairs
7. **IsEmpty**: Check if map is empty
8. **Clear**: Remove all entries

### Additional Operations (Tree Map)
1. **First Key**: Get smallest key
2. **Last Key**: Get largest key
3. **Floor Key**: Largest key ≤ given key
4. **Ceiling Key**: Smallest key ≥ given key
5. **Range Query**: Get entries in key range

## Time Complexity

### Hash Map Operations
| Operation | Average Case | Worst Case |
|-----------|--------------|------------|
| Put | O(1) | O(n) |
| Get | O(1) | O(n) |
| Remove | O(1) | O(n) |
| ContainsKey | O(1) | O(n) |
| ContainsValue | O(n) | O(n) |

### Tree Map Operations
| Operation | Time Complexity |
|-----------|----------------|
| Put | O(log n) |
| Get | O(log n) |
| Remove | O(log n) |
| ContainsKey | O(log n) |
| First/Last Key | O(log n) |
| Range Query | O(log n + k) |

## Space Complexity
- O(n) where n is the number of key-value pairs

## Advantages
- Fast key-based lookup
- Flexible key types
- Efficient insertion and deletion
- Key uniqueness guaranteed
- Useful for indexing and caching

## Disadvantages
- No duplicate keys (by design)
- Unordered in hash maps (unless sorted map used)
- Memory overhead
- Slower value-based operations

## Use Cases
- Database indexing
- Caching and memoization
- Counting frequencies
- Symbol tables in compilers
- Configuration storage
- Implementing associative arrays
- Grouping data by key
- Implementing sets (using map with dummy values)
- Phone directories
- JSON-like data structures

## Example Applications

### Frequency Counting
```python
# Count word frequencies
word_count = {}
for word in words:
    word_count[word] = word_count.get(word, 0) + 1
# Result: {"apple": 3, "banana": 2, ...}
```

### Caching
```python
# Memoization
cache = {}
def fibonacci(n):
    if n in cache:
        return cache[n]  # O(1) lookup
    if n <= 1:
        return n
    result = fibonacci(n-1) + fibonacci(n-2)
    cache[n] = result
    return result
```

### Grouping Data
```python
# Group students by grade
students_by_grade = {}
for student in students:
    grade = student.grade
    if grade not in students_by_grade:
        students_by_grade[grade] = []
    students_by_grade[grade].append(student)
```

### Two Sum Problem
```python
# Find two numbers that sum to target
def two_sum(nums, target):
    seen = {}
    for i, num in enumerate(nums):
        complement = target - num
        if complement in seen:
            return [seen[complement], i]
        seen[num] = i
```

## Implementation Details

### Hash Map Implementation
- Uses hash table internally
- Hash function maps key to bucket
- Handles collisions using chaining or open addressing
- Load factor triggers resizing

### Tree Map Implementation
- Uses balanced BST (typically Red-Black tree)
- Maintains keys in sorted order
- Supports ordered traversal
- More memory overhead than hash map

## Common Patterns

### Default Value Pattern
```python
# Initialize with default value
count = {}
for item in items:
    count[item] = count.get(item, 0) + 1
```

### Dictionary as Switch Statement
```python
# Map values to functions
operations = {
    'add': lambda x, y: x + y,
    'subtract': lambda x, y: x - y,
    'multiply': lambda x, y: x * y
}
result = operations[op](a, b)
```

### Invert Dictionary
```python
# Swap keys and values
original = {'a': 1, 'b': 2, 'c': 3}
inverted = {v: k for k, v in original.items()}
```

## Common Problems
- Two Sum
- Group Anagrams
- Longest Substring Without Repeating Characters
- Contains Duplicate
- Design HashMap
- LRU Cache (uses map + doubly linked list)
- First Unique Character
- Word Pattern
- Top K Frequent Elements
- Valid Anagram

## When to Use Hash Map vs Tree Map

### Use Hash Map When:
- Order doesn't matter
- Need fastest average-case performance
- Just need key-value storage and lookup
- Don't need range queries

### Use Tree Map When:
- Need keys in sorted order
- Need range queries
- Need to find min/max key efficiently
- Want predictable O(log n) performance
- Need to iterate in sorted order

## Best Practices
- Use maps for key-value associations
- Choose appropriate map type based on ordering needs
- Handle missing keys gracefully (get with default)
- Be aware of hashable requirement for keys (hash maps)
- Consider memory overhead vs performance
- Use maps for O(1) lookups instead of nested loops
- Consider thread safety for concurrent access

## Language-Specific Notes

### Python
- `dict`: Hash map implementation
- Dictionary comprehensions supported
- Ordered insertion (Python 3.7+)

### Java
- `HashMap`: Hash map implementation
- `TreeMap`: Sorted map implementation
- `LinkedHashMap`: Maintains insertion order

### C++
- `unordered_map`: Hash map
- `map`: Tree map (sorted)
- `multimap`: Allows duplicate keys

### JavaScript
- Objects: Can be used as maps (string keys)
- `Map`: Built-in map data structure
- Preserves insertion order


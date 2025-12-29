# Tries (Prefix Trees)

## Definition
A trie (pronounced "try"), also called a prefix tree, is a tree-like data structure used to store a dynamic set of strings where the keys are usually strings. The word "trie" comes from the word "retrieval."

## Characteristics
- **Prefix-Based Structure**: Common prefixes share the same path
- **Tree Structure**: Root to leaf path represents a string
- **Efficient String Operations**: Fast prefix matching and search
- **Memory Efficient**: Shares common prefixes among strings
- **Character-Based Nodes**: Each node typically represents a character

## Structure

### Node Representation
```
Trie Node:
  - Character value (optional, can be inferred from position)
  - Children: Array/map of child nodes (one per character)
  - isEndOfWord: Boolean flag indicating end of word
  - Additional data: Can store associated values
```

### Example
```
Strings: "cat", "car", "can", "dog", "dot"

        (root)
       /      \
      c        d
     /          \
    a            o
   /|\           /\
  t r n        g  t
  | | |
  * * *        *  *

* = end of word
```

## Operations

### Core Operations
1. **Insert**: Add a string to the trie
2. **Search**: Check if a string exists (exact match)
3. **StartsWith**: Check if any string has a given prefix
4. **Delete**: Remove a string from the trie
5. **Auto-complete**: Find all strings with given prefix
6. **Longest Common Prefix**: Find longest prefix among strings

## Time Complexity

| Operation | Time Complexity | Description |
|-----------|----------------|-------------|
| Insert | O(m) | m = length of string |
| Search | O(m) | m = length of string |
| StartsWith | O(m) | m = length of prefix |
| Delete | O(m) | m = length of string |
| Auto-complete | O(m + k) | m = prefix length, k = results |

## Space Complexity
- O(ALPHABET_SIZE * N * M)
- Where N = number of strings, M = average length
- Can be optimized with compressed tries or ternary search trees

## Advantages
- Fast prefix matching (O(m) where m = prefix length)
- Efficient string search
- Automatic sorting (in-order traversal gives sorted strings)
- Memory efficient for common prefixes
- Useful for auto-complete features
- Handles string prefixes naturally

## Disadvantages
- High memory usage (sparse nodes)
- Not suitable for large alphabets
- Slower than hash tables for exact match
- Complex implementation
- Cache-unfriendly (pointer-heavy structure)

## Use Cases
- Auto-complete and search suggestions
- Spell checkers and dictionaries
- IP routing (longest prefix matching)
- Phone directory applications
- Search engines (prefix matching)
- Bioinformatics (DNA sequence matching)
- Network routers
- Predictive text input
- Contact list search

## Example Applications

### Auto-complete
```python
# User types "ca"
trie.startsWith("ca")
# Returns: ["cat", "car", "can"]
```

### Dictionary Search
```python
# Check if word exists
trie.search("cat")    # True
trie.search("cab")    # False
```

### Longest Prefix Matching
```python
# Network routing
IP: "192.168.1.1"
Find: Longest matching prefix route
```

## Variations

### 1. Compressed Trie (Radix Tree)
- Merge nodes with single child
- Reduces memory usage
- More complex implementation

### 2. Ternary Search Trie (TST)
- Three-way branching (left, middle, right)
- More memory efficient
- Trade-off between trie and BST

### 3. Suffix Tree
- Stores all suffixes of a string
- Used for pattern matching
- More complex structure

## Implementation Considerations

### Node Structure
```python
class TrieNode:
    def __init__(self):
        self.children = {}  # Dictionary for children
        self.is_end_of_word = False
        self.value = None  # Optional: store associated value
```

### Alphabet Size
- **Small Alphabet**: Array of fixed size (26 for lowercase English)
- **Large Alphabet**: Hash map/dictionary for children
- Trade-off between memory and flexibility

### Memory Optimization
- Use compressed tries
- Implement lazy deletion
- Consider TST for space efficiency
- Use bit packing for small alphabets

## Common Operations Implementation

### Insert
1. Start from root
2. For each character in string:
   - If child node doesn't exist, create it
   - Move to child node
3. Mark last node as end of word

### Search
1. Start from root
2. For each character in string:
   - If child node doesn't exist, return False
   - Move to child node
3. Check if last node is marked as end of word

### Prefix Search
1. Traverse to prefix end
2. Perform DFS from that node
3. Collect all words with that prefix

## Common Problems
- Implement Trie (Prefix Tree)
- Add and Search Word
- Word Search II
- Longest Word in Dictionary
- Replace Words
- Maximum XOR of Two Numbers
- Design Search Autocomplete System
- Prefix Matching
- Word Break (can use trie)

## Comparison with Other Structures

### Trie vs Hash Table
- **Trie**: Better for prefix matching, sorted order
- **Hash Table**: Faster exact match, simpler

### Trie vs BST
- **Trie**: Better for string operations, prefix search
- **BST**: General purpose, better for numeric keys

## Best Practices
- Choose appropriate node structure based on alphabet size
- Implement cleanup for deleted nodes (if needed)
- Consider memory vs speed trade-offs
- Use compressed tries for large datasets
- Handle edge cases (empty strings, null values)
- Optimize for specific use case (search-heavy vs insert-heavy)


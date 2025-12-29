# Stacks

## Definition
A stack is a linear data structure that follows the Last In First Out (LIFO) principle. Elements are added and removed from the same end, called the "top" of the stack.

## Characteristics
- **LIFO Principle**: Last element added is the first one to be removed
- **Single Access Point**: Only the top element is accessible
- **Limited Operations**: Push, Pop, Peek/Top operations
- **Linear Structure**: Elements are arranged in a linear order

## Operations

### Core Operations
1. **Push**: Add an element to the top of the stack
2. **Pop**: Remove and return the top element
3. **Peek/Top**: View the top element without removing it
4. **isEmpty**: Check if the stack is empty
5. **Size**: Get the number of elements in the stack

## Time Complexity

| Operation | Time Complexity | Description |
|-----------|----------------|-------------|
| Push | O(1) | Add element at top |
| Pop | O(1) | Remove top element |
| Peek | O(1) | Access top element |
| Search | O(n) | Search for an element |
| isEmpty | O(1) | Check if empty |

## Space Complexity
- O(n) where n is the number of elements

## Implementation Methods

### 1. Array-based Implementation
- Use array/vector to store elements
- Track top index
- Fixed or dynamic size

### 2. Linked List Implementation
- Use linked list with insertion/deletion at head
- Dynamic size, no overflow issues

## Advantages
- Simple and efficient implementation
- Fast operations (O(1) for push/pop)
- Memory efficient
- Easy to understand

## Disadvantages
- Limited access (only top element)
- No random access
- Can overflow (in array-based implementation)

## Use Cases
- Expression evaluation and syntax parsing
- Function call management (call stack)
- Undo/Redo operations in applications
- Backtracking algorithms
- Bracket matching and validation
- Browser history (back button)
- Depth-First Search (DFS) algorithm

## Example Applications

### Expression Evaluation
```
Infix: (A + B) * C
Postfix: AB+C*
Stack is used to convert and evaluate expressions
```

### Function Calls
```
When function A calls function B:
1. Push A's state to stack
2. Execute B
3. Pop A's state and continue
```

### Bracket Matching
```
Check if brackets are balanced:
([{}]) → Valid
([{]) → Invalid (stack helps identify mismatched brackets)
```

## Common Problems
- Valid Parentheses
- Next Greater Element
- Largest Rectangle in Histogram
- Stock Span Problem
- Infix to Postfix Conversion
- Reverse a string using stack


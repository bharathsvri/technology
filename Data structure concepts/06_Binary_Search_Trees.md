# Binary Search Trees (BST)

## Definition
A Binary Search Tree (BST) is a binary tree data structure where each node has a value, and the values in the left subtree are less than the node's value, while values in the right subtree are greater than the node's value.

## Characteristics
- **Ordering Property**: Left child < Parent < Right child
- **No Duplicates**: Typically, each value appears only once (some implementations allow duplicates)
- **Binary Structure**: Each node has at most 2 children
- **Recursive Structure**: Both subtrees are also BSTs

## BST Property
For any node with value `v`:
- All nodes in left subtree have values < `v`
- All nodes in right subtree have values > `v`
- This property holds recursively for all nodes

## Operations

### 1. Search
- Start from root
- Compare target with current node
- If equal: found
- If less: go left
- If greater: go right
- Continue until found or reach null

### 2. Insert
- Start from root
- Find appropriate position following BST property
- Insert new node at leaf position
- Maintain BST ordering

### 3. Delete
Three cases:
- **Leaf node**: Simply remove it
- **One child**: Replace node with its child
- **Two children**: Replace with inorder successor/predecessor

### 4. Traversal
- **Inorder**: Returns sorted order (left, root, right)
- **Preorder**: Root first (root, left, right)
- **Postorder**: Root last (left, right, root)

## Time Complexity

| Operation | Average Case | Worst Case |
|-----------|--------------|------------|
| Search | O(log n) | O(n) |
| Insert | O(log n) | O(n) |
| Delete | O(log n) | O(n) |
| Find Min/Max | O(log n) | O(n) |
| Traversal | O(n) | O(n) |

**Note**: Worst case occurs when tree is skewed (like a linked list)

## Space Complexity
- O(n) for storing n nodes
- O(h) for recursion stack where h is height
- O(log n) average, O(n) worst case

## Advantages
- Efficient search operations (average O(log n))
- Maintains sorted order
- Efficient insertion and deletion
- Supports range queries
- Flexible size (dynamic)

## Disadvantages
- Worst case O(n) if tree is unbalanced
- No guarantee of balance
- Performance degrades with skewed trees
- More complex than arrays for simple operations

## Balancing

### Why Balance?
Unbalanced BST (skewed) → O(n) operations
Balanced BST → O(log n) operations

### Self-Balancing BSTs
1. **AVL Tree**: Height-balanced (difference ≤ 1)
2. **Red-Black Tree**: Color-based balancing
3. **Splay Tree**: Self-adjusting
4. **Treap**: Combination of BST and heap

## Example Operations

### Insertion Example
```
Insert: 5, 3, 7, 2, 4, 6, 8

Result:
        5
       / \
      3   7
     / \ / \
    2  4 6  8
```

### Search Example
```
Search for 4:
Start at 5 → 4 < 5, go left
At 3 → 4 > 3, go right
Found 4!
```

### Deletion Example
```
Delete node with value 3:
Case: Node has two children (2 and 4)
Replace 3 with inorder successor (4)
```

## Special Operations

### Successor/Predecessor
- **Successor**: Next largest element
- **Predecessor**: Next smallest element

### Range Queries
- Find all elements between min and max
- Efficient with BST structure

### Find Minimum/Maximum
- **Min**: Keep going left until null
- **Max**: Keep going right until null

## Use Cases
- Database indexing
- Maintaining sorted data
- Range queries
- Symbol tables
- Priority queues (when combined with heap)
- Implementing sets and maps
- Expression evaluation

## Common Problems
- Validate BST
- BST Iterator
- Kth Smallest Element
- Lowest Common Ancestor in BST
- Convert Sorted Array to BST
- Delete Node in BST
- Range Sum of BST
- Inorder Successor in BST

## Implementation Considerations
- Handle duplicate values (store count or ignore)
- Use recursive or iterative approach
- Consider thread-safe implementation for concurrent access
- Balance tree for guaranteed O(log n) performance


# Trees

## Definition
A tree is a hierarchical data structure consisting of nodes connected by edges. It is a non-linear data structure where each node has a parent (except root) and zero or more children.

## Characteristics
- **Hierarchical Structure**: Organized in levels
- **Single Root**: One root node at the top
- **No Cycles**: Acyclic graph (no circular paths)
- **Parent-Child Relationship**: Each node (except root) has exactly one parent
- **Connected**: There is a path from root to any node

## Terminology

### Node Components
- **Root**: Topmost node (no parent)
- **Parent**: Node that has children
- **Child**: Node connected to a parent
- **Leaf**: Node with no children
- **Internal Node**: Node with at least one child
- **Sibling**: Nodes with the same parent

### Tree Properties
- **Depth**: Number of edges from root to node
- **Height**: Maximum depth of any node
- **Level**: Nodes at the same depth
- **Degree**: Number of children a node has
- **Path**: Sequence of nodes from one node to another

## Types of Trees

### 1. Binary Tree
- Each node has at most 2 children (left and right)
- No ordering constraint

### 2. Binary Search Tree (BST)
- Left child < Parent < Right child
- Enables efficient search operations

### 3. AVL Tree
- Self-balancing BST
- Height difference between subtrees ≤ 1

### 4. Red-Black Tree
- Self-balancing BST
- Uses color coding for balancing

### 5. B-Tree
- Multi-way search tree
- Used in databases and file systems

### 6. Trie (Prefix Tree)
- Specialized tree for strings
- Each path represents a prefix

### 7. Heap
- Complete binary tree
- Parent-child ordering maintained

## Tree Traversals

### Depth-First Search (DFS)
1. **Inorder** (Left, Root, Right)
   - For BST: Returns sorted order
2. **Preorder** (Root, Left, Right)
   - Used for creating tree copy
3. **Postorder** (Left, Right, Root)
   - Used for deleting tree

### Breadth-First Search (BFS)
- Level-order traversal
- Visit nodes level by level

## Time Complexity

| Operation | Binary Tree | BST | Balanced BST |
|-----------|-------------|-----|--------------|
| Search | O(n) | O(n) worst, O(log n) avg | O(log n) |
| Insert | O(n) | O(n) worst, O(log n) avg | O(log n) |
| Delete | O(n) | O(n) worst, O(log n) avg | O(log n) |
| Traversal | O(n) | O(n) | O(n) |

## Space Complexity
- O(n) for storing n nodes
- O(h) for recursion stack (h = height)

## Advantages
- Hierarchical representation
- Efficient searching (in BST)
- Fast insertion/deletion (balanced trees)
- Flexible structure
- Natural representation for hierarchical data

## Disadvantages
- Complex implementation
- Unbalanced trees degrade performance
- Requires balancing in BST for optimal performance
- More memory overhead than arrays

## Use Cases
- File systems (directory structure)
- Database indexing (B-trees)
- Expression trees (compiler design)
- Decision trees (machine learning)
- Organizational charts
- XML/HTML parsing
- Heap for priority queues
- Tries for autocomplete
- Syntax trees in compilers

## Example Structure
```
        A
       / \
      B   C
     / \   \
    D   E   F
           / \
          G   H

Root: A
Leaves: D, E, G, H
Height: 3
Depth of G: 3
```

## Common Problems
- Binary Tree Level Order Traversal
- Maximum Depth of Binary Tree
- Validate Binary Search Tree
- Lowest Common Ancestor
- Invert Binary Tree
- Serialize/Deserialize Binary Tree
- Path Sum problems


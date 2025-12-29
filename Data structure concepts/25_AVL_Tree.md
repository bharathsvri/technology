# AVL Tree

## Definition
An AVL (Adelson-Velsky and Landis) tree is a self-balancing binary search tree where the difference between heights of left and right subtrees (balance factor) cannot be more than 1 for all nodes.

## Properties
- **Height Balanced**: Balance factor ≤ 1 for all nodes
- **Search Time**: O(log n) guaranteed
- **Balance Factor**: height(left) - height(right)
- **Rotations**: Used to maintain balance

## Balance Factor
```
Balance Factor = height(left subtree) - height(right subtree)

Valid values: -1, 0, 1
If |balance factor| > 1, tree needs rotation
```

## Rotations

### 1. Left Rotation (Right-Right Case)
```
    A              B
   / \            / \
  T1  B    =>    A  T3
     / \        / \
    T2  T3     T1 T2
```

### 2. Right Rotation (Left-Left Case)
```
      A            B
     / \          / \
    B  T3   =>   T1  A
   / \              / \
  T1 T2            T2 T3
```

### 3. Left-Right Rotation (Left-Right Case)
First left rotate on left child, then right rotate on root.

### 4. Right-Left Rotation (Right-Left Case)
First right rotate on right child, then left rotate on root.

## Java Implementation

```java
class AVLNode {
    int key;
    int height;
    AVLNode left;
    AVLNode right;
    
    AVLNode(int key) {
        this.key = key;
        this.height = 1;
        this.left = null;
        this.right = null;
    }
}

public class AVLTree {
    private AVLNode root;
    
    // Get height of node
    private int height(AVLNode node) {
        if (node == null) return 0;
        return node.height;
    }
    
    // Get balance factor
    private int getBalance(AVLNode node) {
        if (node == null) return 0;
        return height(node.left) - height(node.right);
    }
    
    // Right rotate
    private AVLNode rightRotate(AVLNode y) {
        AVLNode x = y.left;
        AVLNode T2 = x.right;
        
        // Perform rotation
        x.right = y;
        y.left = T2;
        
        // Update heights
        y.height = Math.max(height(y.left), height(y.right)) + 1;
        x.height = Math.max(height(x.left), height(x.right)) + 1;
        
        return x;
    }
    
    // Left rotate
    private AVLNode leftRotate(AVLNode x) {
        AVLNode y = x.right;
        AVLNode T2 = y.left;
        
        // Perform rotation
        y.left = x;
        x.right = T2;
        
        // Update heights
        x.height = Math.max(height(x.left), height(x.right)) + 1;
        y.height = Math.max(height(y.left), height(y.right)) + 1;
        
        return y;
    }
    
    // Insert node
    public void insert(int key) {
        root = insert(root, key);
    }
    
    private AVLNode insert(AVLNode node, int key) {
        // 1. Perform normal BST insertion
        if (node == null) {
            return new AVLNode(key);
        }
        
        if (key < node.key) {
            node.left = insert(node.left, key);
        } else if (key > node.key) {
            node.right = insert(node.right, key);
        } else {
            return node; // Duplicate keys not allowed
        }
        
        // 2. Update height
        node.height = 1 + Math.max(height(node.left), height(node.right));
        
        // 3. Get balance factor
        int balance = getBalance(node);
        
        // 4. If unbalanced, perform rotations
        
        // Left Left Case
        if (balance > 1 && key < node.left.key) {
            return rightRotate(node);
        }
        
        // Right Right Case
        if (balance < -1 && key > node.right.key) {
            return leftRotate(node);
        }
        
        // Left Right Case
        if (balance > 1 && key > node.left.key) {
            node.left = leftRotate(node.left);
            return rightRotate(node);
        }
        
        // Right Left Case
        if (balance < -1 && key < node.right.key) {
            node.right = rightRotate(node.right);
            return leftRotate(node);
        }
        
        return node;
    }
    
    // Delete node
    public void delete(int key) {
        root = delete(root, key);
    }
    
    private AVLNode delete(AVLNode root, int key) {
        // 1. Perform standard BST delete
        if (root == null) return root;
        
        if (key < root.key) {
            root.left = delete(root.left, key);
        } else if (key > root.key) {
            root.right = delete(root.right, key);
        } else {
            // Node with one or no child
            if (root.left == null) {
                return root.right;
            } else if (root.right == null) {
                return root.left;
            }
            
            // Node with two children: get inorder successor
            root.key = minValue(root.right);
            root.right = delete(root.right, root.key);
        }
        
        // 2. Update height
        root.height = Math.max(height(root.left), height(root.right)) + 1;
        
        // 3. Get balance factor
        int balance = getBalance(root);
        
        // 4. If unbalanced, perform rotations
        
        // Left Left Case
        if (balance > 1 && getBalance(root.left) >= 0) {
            return rightRotate(root);
        }
        
        // Left Right Case
        if (balance > 1 && getBalance(root.left) < 0) {
            root.left = leftRotate(root.left);
            return rightRotate(root);
        }
        
        // Right Right Case
        if (balance < -1 && getBalance(root.right) <= 0) {
            return leftRotate(root);
        }
        
        // Right Left Case
        if (balance < -1 && getBalance(root.right) > 0) {
            root.right = rightRotate(root.right);
            return leftRotate(root);
        }
        
        return root;
    }
    
    private int minValue(AVLNode node) {
        while (node.left != null) {
            node = node.left;
        }
        return node.key;
    }
    
    // Search
    public boolean search(int key) {
        return search(root, key);
    }
    
    private boolean search(AVLNode node, int key) {
        if (node == null) return false;
        if (key == node.key) return true;
        if (key < node.key) return search(node.left, key);
        return search(node.right, key);
    }
    
    // Inorder traversal
    public void inorder() {
        inorder(root);
        System.out.println();
    }
    
    private void inorder(AVLNode node) {
        if (node != null) {
            inorder(node.left);
            System.out.print(node.key + " ");
            inorder(node.right);
        }
    }
}
```

## Time Complexity

| Operation | Time Complexity |
|-----------|----------------|
| Search | O(log n) |
| Insert | O(log n) |
| Delete | O(log n) |
| Rotation | O(1) |

## Space Complexity
- O(n) for storing n nodes
- O(log n) for recursion stack

## Advantages
- Guaranteed O(log n) height
- Better performance than unbalanced BST
- Search operations are fast

## Disadvantages
- More complex than BST
- Additional storage for height
- Slightly slower insert/delete due to rotations

## Use Cases
- Database indexing
- Memory management
- Implementing ordered maps
- When guaranteed O(log n) performance needed


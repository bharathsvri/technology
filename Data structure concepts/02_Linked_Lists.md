# Linked Lists (Java Implementation)

## Definition
A linked list is a linear data structure where elements (nodes) are stored in non-contiguous memory locations. Each node contains data and a reference (pointer) to the next node in the sequence.

## Characteristics
- **Dynamic Size**: Can grow or shrink at runtime
- **Non-contiguous Memory**: Nodes are not stored in consecutive memory locations
- **Node Structure**: Each node contains data and a pointer to the next node
- **Sequential Access**: Elements must be accessed sequentially (no random access)

## Types of Linked Lists

### 1. Singly Linked List
- Each node points only to the next node
- Unidirectional traversal

### 2. Doubly Linked List
- Each node points to both next and previous nodes
- Bidirectional traversal

### 3. Circular Linked List
- Last node points back to the first node
- Can be singly or doubly circular

## Operations and Time Complexity

| Operation | Time Complexity | Description |
|-----------|----------------|-------------|
| Access | O(n) | Must traverse from head to target |
| Search | O(n) | Linear search through the list |
| Insertion (beginning) | O(1) | Insert at head |
| Insertion (end) | O(n) or O(1)* | *O(1) if tail pointer maintained |
| Insertion (middle) | O(n) | Find position + insert |
| Deletion (beginning) | O(1) | Remove head node |
| Deletion (end) | O(n) | Find last node + delete |
| Deletion (middle) | O(n) | Find node + delete |

## Space Complexity
- O(n) where n is the number of nodes
- Each node stores data + pointer(s)

## Advantages
- Dynamic size (no wasted memory)
- Efficient insertion/deletion at beginning
- No need to pre-allocate memory
- Easy to add/remove elements

## Disadvantages
- No random access (must traverse from head)
- Extra memory for pointers
- Not cache-friendly (non-contiguous memory)
- Slower access time compared to arrays

## Use Cases
- Implementing stacks and queues
- When frequent insertions/deletions are needed
- Memory management (dynamic allocation)
- Polynomial representation
- Browser history (back/forward navigation)
- Music playlists

## Java Implementation

### Singly Linked List Node
```java
class ListNode {
    int val;
    ListNode next;
    
    ListNode(int val) {
        this.val = val;
        this.next = null;
    }
    
    ListNode(int val, ListNode next) {
        this.val = val;
        this.next = next;
    }
}
```

### Singly Linked List Implementation
```java
public class SinglyLinkedList {
    private ListNode head;
    private int size;
    
    public SinglyLinkedList() {
        head = null;
        size = 0;
    }
    
    // Insert at beginning
    public void insertAtHead(int val) {
        ListNode newNode = new ListNode(val);
        newNode.next = head;
        head = newNode;
        size++;
    }
    
    // Insert at end
    public void insertAtTail(int val) {
        ListNode newNode = new ListNode(val);
        if (head == null) {
            head = newNode;
        } else {
            ListNode current = head;
            while (current.next != null) {
                current = current.next;
            }
            current.next = newNode;
        }
        size++;
    }
    
    // Insert at position
    public void insertAtPosition(int val, int position) {
        if (position < 0 || position > size) {
            throw new IndexOutOfBoundsException("Invalid position");
        }
        
        if (position == 0) {
            insertAtHead(val);
            return;
        }
        
        ListNode newNode = new ListNode(val);
        ListNode current = head;
        for (int i = 0; i < position - 1; i++) {
            current = current.next;
        }
        newNode.next = current.next;
        current.next = newNode;
        size++;
    }
    
    // Delete from beginning
    public void deleteAtHead() {
        if (head == null) {
            throw new IllegalStateException("List is empty");
        }
        head = head.next;
        size--;
    }
    
    // Delete by value
    public void deleteByValue(int val) {
        if (head == null) {
            return;
        }
        
        if (head.val == val) {
            head = head.next;
            size--;
            return;
        }
        
        ListNode current = head;
        while (current.next != null && current.next.val != val) {
            current = current.next;
        }
        
        if (current.next != null) {
            current.next = current.next.next;
            size--;
        }
    }
    
    // Search
    public boolean search(int val) {
        ListNode current = head;
        while (current != null) {
            if (current.val == val) {
                return true;
            }
            current = current.next;
        }
        return false;
    }
    
    // Get size
    public int size() {
        return size;
    }
    
    // Check if empty
    public boolean isEmpty() {
        return head == null;
    }
    
    // Print list
    public void print() {
        ListNode current = head;
        while (current != null) {
            System.out.print(current.val + " -> ");
            current = current.next;
        }
        System.out.println("null");
    }
}
```

### Doubly Linked List Node
```java
class DoublyListNode {
    int val;
    DoublyListNode next;
    DoublyListNode prev;
    
    DoublyListNode(int val) {
        this.val = val;
        this.next = null;
        this.prev = null;
    }
}
```

### Doubly Linked List Implementation
```java
public class DoublyLinkedList {
    private DoublyListNode head;
    private DoublyListNode tail;
    private int size;
    
    public DoublyLinkedList() {
        head = null;
        tail = null;
        size = 0;
    }
    
    // Insert at beginning
    public void insertAtHead(int val) {
        DoublyListNode newNode = new DoublyListNode(val);
        if (head == null) {
            head = tail = newNode;
        } else {
            newNode.next = head;
            head.prev = newNode;
            head = newNode;
        }
        size++;
    }
    
    // Insert at end
    public void insertAtTail(int val) {
        DoublyListNode newNode = new DoublyListNode(val);
        if (tail == null) {
            head = tail = newNode;
        } else {
            tail.next = newNode;
            newNode.prev = tail;
            tail = newNode;
        }
        size++;
    }
    
    // Delete from beginning
    public void deleteAtHead() {
        if (head == null) {
            throw new IllegalStateException("List is empty");
        }
        
        if (head == tail) {
            head = tail = null;
        } else {
            head = head.next;
            head.prev = null;
        }
        size--;
    }
    
    // Delete from end
    public void deleteAtTail() {
        if (tail == null) {
            throw new IllegalStateException("List is empty");
        }
        
        if (head == tail) {
            head = tail = null;
        } else {
            tail = tail.prev;
            tail.next = null;
        }
        size--;
    }
    
    // Delete by value
    public void deleteByValue(int val) {
        DoublyListNode current = head;
        while (current != null) {
            if (current.val == val) {
                if (current == head) {
                    deleteAtHead();
                } else if (current == tail) {
                    deleteAtTail();
                } else {
                    current.prev.next = current.next;
                    current.next.prev = current.prev;
                    size--;
                }
                return;
            }
            current = current.next;
        }
    }
    
    public int size() {
        return size;
    }
    
    public boolean isEmpty() {
        return head == null;
    }
}
```

### Common Operations

#### Reverse Linked List
```java
public static ListNode reverseList(ListNode head) {
    ListNode prev = null;
    ListNode current = head;
    
    while (current != null) {
        ListNode next = current.next;
        current.next = prev;
        prev = current;
        current = next;
    }
    
    return prev;
}

// Recursive version
public static ListNode reverseListRecursive(ListNode head) {
    if (head == null || head.next == null) {
        return head;
    }
    
    ListNode reversed = reverseListRecursive(head.next);
    head.next.next = head;
    head.next = null;
    
    return reversed;
}
```

#### Detect Cycle
```java
public static boolean hasCycle(ListNode head) {
    if (head == null || head.next == null) {
        return false;
    }
    
    ListNode slow = head;
    ListNode fast = head;
    
    while (fast != null && fast.next != null) {
        slow = slow.next;
        fast = fast.next.next;
        if (slow == fast) {
            return true;
        }
    }
    
    return false;
}
```

#### Find Middle Node
```java
public static ListNode findMiddle(ListNode head) {
    ListNode slow = head;
    ListNode fast = head;
    
    while (fast != null && fast.next != null) {
        slow = slow.next;
        fast = fast.next.next;
    }
    
    return slow;
}
```

#### Merge Two Sorted Lists
```java
public static ListNode mergeTwoLists(ListNode list1, ListNode list2) {
    ListNode dummy = new ListNode(0);
    ListNode current = dummy;
    
    while (list1 != null && list2 != null) {
        if (list1.val <= list2.val) {
            current.next = list1;
            list1 = list1.next;
        } else {
            current.next = list2;
            list2 = list2.next;
        }
        current = current.next;
    }
    
    if (list1 != null) {
        current.next = list1;
    }
    if (list2 != null) {
        current.next = list2;
    }
    
    return dummy.next;
}
```

#### Remove Nth Node From End
```java
public static ListNode removeNthFromEnd(ListNode head, int n) {
    ListNode dummy = new ListNode(0);
    dummy.next = head;
    
    ListNode first = dummy;
    ListNode second = dummy;
    
    // Move first pointer n+1 steps ahead
    for (int i = 0; i <= n; i++) {
        first = first.next;
    }
    
    // Move both until first reaches end
    while (first != null) {
        first = first.next;
        second = second.next;
    }
    
    // Remove node
    second.next = second.next.next;
    
    return dummy.next;
}
```

## Common Patterns
- **Two-pointer technique**: Fast and slow pointers for cycle detection
- **Dummy head node**: Simplifies edge cases in insertion/deletion
- **Reverse linked list**: Common interview problem

## Common Problems
- Reverse Linked List
- Merge Two Sorted Lists
- Remove Nth Node From End
- Linked List Cycle
- Middle of Linked List
- Palindrome Linked List
- Intersection of Two Linked Lists
- Add Two Numbers
- Copy List with Random Pointer

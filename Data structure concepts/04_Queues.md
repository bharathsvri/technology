# Queues

## Definition
A queue is a linear data structure that follows the First In First Out (FIFO) principle. Elements are added at the rear (enqueue) and removed from the front (dequeue).

## Characteristics
- **FIFO Principle**: First element added is the first one to be removed
- **Two Access Points**: Front (for deletion) and Rear (for insertion)
- **Ordered Collection**: Maintains the order of insertion
- **Linear Structure**: Elements are arranged sequentially

## Operations

### Core Operations
1. **Enqueue**: Add an element to the rear of the queue
2. **Dequeue**: Remove and return the front element
3. **Front/Peek**: View the front element without removing it
4. **Rear**: View the rear element
5. **isEmpty**: Check if the queue is empty
6. **Size**: Get the number of elements in the queue

## Time Complexity

| Operation | Time Complexity | Description |
|-----------|----------------|-------------|
| Enqueue | O(1) | Add element at rear |
| Dequeue | O(1) | Remove front element |
| Front | O(1) | Access front element |
| Rear | O(1) | Access rear element |
| Search | O(n) | Search for an element |
| isEmpty | O(1) | Check if empty |

## Space Complexity
- O(n) where n is the number of elements

## Implementation Methods

### 1. Array-based Implementation
- Use circular array to efficiently utilize space
- Maintain front and rear pointers
- Handle overflow and underflow conditions

### 2. Linked List Implementation
- Use linked list with insertion at tail and deletion at head
- Dynamic size, no overflow issues

## Types of Queues

### 1. Simple Queue
- Basic FIFO structure
- Single-ended queue

### 2. Circular Queue
- Rear connects back to front
- Better memory utilization
- Prevents space wastage

### 3. Priority Queue
- Elements have priority values
- Higher priority elements dequeued first
- Implemented using heap

### 4. Double-ended Queue (Deque)
- Insertion and deletion at both ends
- More flexible than standard queue

## Advantages
- Simple and efficient implementation
- Fast operations (O(1) for enqueue/dequeue)
- Maintains insertion order
- Useful for scheduling problems

## Disadvantages
- Limited access (only front and rear)
- No random access
- Search operation is O(n)

## Use Cases
- Process scheduling in operating systems
- Printer job scheduling
- Breadth-First Search (BFS) algorithm
- Call center phone systems
- Message queues in distributed systems
- Task scheduling
- CPU scheduling
- Traffic management

## Example Applications

### BFS (Breadth-First Search)
```
Queue helps traverse graph level by level:
1. Enqueue starting node
2. While queue not empty:
   - Dequeue node
   - Enqueue all unvisited neighbors
```

### Printer Queue
```
Print jobs are added to queue in order:
Job 1 → Job 2 → Job 3
First job gets printed first (FIFO)
```

### CPU Scheduling
```
Processes waiting for CPU execution:
Ready Queue maintains process order
First process in queue gets CPU first
```

## Common Problems
- Sliding Window Maximum
- First Non-repeating Character in Stream
- Generate Binary Numbers
- Queue using Stacks
- Interleave First Half with Second Half


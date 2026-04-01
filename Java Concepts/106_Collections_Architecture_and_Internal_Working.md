# Collections Architecture and Internal Working

## Why Collections?
The Java Collections Framework provides reusable data structures and algorithms for storing and processing groups of objects.

## High-Level Hierarchy
```text
Iterable
  -> Collection
     -> List
     -> Set
     -> Queue

Map (separate hierarchy, not a subtype of Collection)
```

## Core Interfaces
- `List`: ordered, allows duplicates
- `Set`: no duplicates
- `Queue`: typically FIFO or priority-based ordering
- `Map`: key-value pairs, unique keys

## Internal Working by Implementation

### ArrayList
- Backed by dynamic array
- Fast random access: O(1)
- Insert/delete in middle: O(n)
- Grows by resizing array when full

### LinkedList
- Doubly linked list
- Fast insert/delete at ends: O(1)
- Random access slow: O(n)
- Also implements `Deque`

### HashSet
- Built on top of `HashMap`
- Stores only keys, value is a dummy constant
- Uses `hashCode()` + `equals()` for uniqueness
- Average add/search/remove: O(1)

### TreeSet
- Backed by `TreeMap` (Red-Black Tree)
- Sorted and unique elements
- Operations: O(log n)

### HashMap (Most Asked)
- Array of buckets (table)
- Each bucket stores entries by hash
- Collision handling:
  - Linked list (older style)
  - Tree bins (balanced tree) for high-collision buckets in modern Java
- Average put/get/remove: O(1), worst case O(log n) with tree bins
- Resizes when load factor threshold is crossed

### LinkedHashMap
- HashMap + doubly linked list across entries
- Preserves insertion order (or access order)
- Useful for LRU cache patterns

### TreeMap
- Red-Black Tree
- Sorted by key (natural order or custom comparator)
- Operations: O(log n)

### PriorityQueue
- Binary heap internally
- `offer` / `poll`: O(log n)
- `peek`: O(1)

### ArrayDeque
- Resizable circular array
- Fast stack/queue operations at both ends
- Usually better than `Stack` for stack behavior

## Equality and Ordering Contracts
- `equals()` and `hashCode()` must be consistent for hash-based collections
- `compareTo()` / `Comparator` must be consistent for sorted collections

## Fail-Fast Iterators
Most collection iterators are fail-fast and throw `ConcurrentModificationException` when structure changes during iteration (outside iterator methods).

## Thread Safety
- Most standard collections are not synchronized.
- Options:
  - `Collections.synchronizedList(...)`
  - `ConcurrentHashMap`
  - `CopyOnWriteArrayList`
  - Blocking queues from `java.util.concurrent`

## Time Complexity Quick View
- `ArrayList#get`: O(1)
- `ArrayList#add` (amortized): O(1)
- `LinkedList#addFirst`: O(1)
- `HashMap#get/put` (average): O(1)
- `TreeMap#get/put`: O(log n)
- `PriorityQueue#offer/poll`: O(log n)

## Selection Guide
- Frequent random reads -> `ArrayList`
- Frequent insertion/deletion at ends -> `LinkedList` or `ArrayDeque`
- Unique unordered elements -> `HashSet`
- Unique sorted elements -> `TreeSet`
- Key-value fast lookup -> `HashMap`
- Ordered key-value -> `LinkedHashMap` or `TreeMap`
- Thread-safe high concurrency map -> `ConcurrentHashMap`

## Best Practices
1. Program to interface (`List`, `Set`, `Map`) not implementation.
2. Choose collection based on access pattern, not habit.
3. Override `equals()` and `hashCode()` correctly.
4. Prefer `ArrayDeque` over legacy `Stack`.
5. Use immutable collections where possible for safety.

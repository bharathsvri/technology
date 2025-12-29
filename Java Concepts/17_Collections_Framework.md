# Collections Framework

## What is Collections Framework?
The Java Collections Framework provides a set of classes and interfaces for storing and manipulating groups of objects. It includes lists, sets, maps, and queues.

## Collection Hierarchy
```
Collection (Interface)
├── List (Interface)
│   ├── ArrayList
│   ├── LinkedList
│   └── Vector
├── Set (Interface)
│   ├── HashSet
│   ├── LinkedHashSet
│   └── TreeSet
└── Queue (Interface)
    ├── PriorityQueue
    └── ArrayDeque

Map (Interface - separate hierarchy)
├── HashMap
├── LinkedHashMap
├── TreeMap
└── Hashtable
```

## List Interface

### ArrayList
Dynamic array that grows automatically.

```java
import java.util.ArrayList;
import java.util.List;

List<String> list = new ArrayList<>();
list.add("Apple");
list.add("Banana");
list.add("Cherry");

// Access elements
String fruit = list.get(0); // "Apple"

// Iterate
for (String item : list) {
    System.out.println(item);
}

// Remove
list.remove("Banana");
list.remove(0);
```

### LinkedList
Doubly-linked list implementation.

```java
import java.util.LinkedList;

LinkedList<String> list = new LinkedList<>();
list.add("First");
list.add("Last");
list.addFirst("New First");
list.addLast("New Last");

// Remove
list.removeFirst();
list.removeLast();
```

### Vector
Synchronized version of ArrayList (legacy).

```java
import java.util.Vector;

Vector<String> vector = new Vector<>();
vector.add("Element");
```

## Set Interface

### HashSet
Unordered collection, no duplicates, uses hashing.

```java
import java.util.HashSet;
import java.util.Set;

Set<String> set = new HashSet<>();
set.add("Apple");
set.add("Banana");
set.add("Apple"); // Duplicate, ignored

System.out.println(set.size()); // 2
System.out.println(set.contains("Apple")); // true
```

### LinkedHashSet
Maintains insertion order.

```java
import java.util.LinkedHashSet;

LinkedHashSet<String> set = new LinkedHashSet<>();
set.add("First");
set.add("Second");
set.add("Third");
// Maintains order: First, Second, Third
```

### TreeSet
Sorted set, maintains natural ordering.

```java
import java.util.TreeSet;

TreeSet<Integer> set = new TreeSet<>();
set.add(5);
set.add(2);
set.add(8);
set.add(1);
// Automatically sorted: 1, 2, 5, 8
```

## Map Interface

### HashMap
Key-value pairs, no order, no duplicates.

```java
import java.util.HashMap;
import java.util.Map;

Map<String, Integer> map = new HashMap<>();
map.put("Apple", 10);
map.put("Banana", 5);
map.put("Cherry", 8);

// Access
int quantity = map.get("Apple"); // 10

// Check
boolean exists = map.containsKey("Apple"); // true

// Iterate
for (Map.Entry<String, Integer> entry : map.entrySet()) {
    System.out.println(entry.getKey() + ": " + entry.getValue());
}
```

### LinkedHashMap
Maintains insertion order.

```java
import java.util.LinkedHashMap;

LinkedHashMap<String, Integer> map = new LinkedHashMap<>();
map.put("First", 1);
map.put("Second", 2);
// Maintains insertion order
```

### TreeMap
Sorted by keys.

```java
import java.util.TreeMap;

TreeMap<String, Integer> map = new TreeMap<>();
map.put("Zebra", 1);
map.put("Apple", 2);
map.put("Banana", 3);
// Sorted by key: Apple, Banana, Zebra
```

## Queue Interface

### PriorityQueue
Elements ordered by priority.

```java
import java.util.PriorityQueue;
import java.util.Queue;

Queue<Integer> queue = new PriorityQueue<>();
queue.offer(5);
queue.offer(2);
queue.offer(8);
queue.offer(1);

while (!queue.isEmpty()) {
    System.out.println(queue.poll()); // 1, 2, 5, 8
}
```

### ArrayDeque
Double-ended queue.

```java
import java.util.ArrayDeque;
import java.util.Deque;

Deque<String> deque = new ArrayDeque<>();
deque.addFirst("First");
deque.addLast("Last");
deque.removeFirst();
deque.removeLast();
```

## Common Collection Operations

### Adding Elements
```java
List<String> list = new ArrayList<>();
list.add("Element");
list.add(0, "First"); // Add at index
list.addAll(Arrays.asList("A", "B", "C"));
```

### Removing Elements
```java
list.remove("Element");
list.remove(0);
list.removeAll(Arrays.asList("A", "B"));
list.clear(); // Remove all
```

### Checking Elements
```java
boolean contains = list.contains("Element");
boolean isEmpty = list.isEmpty();
int size = list.size();
```

### Converting Collections
```java
// List to Array
String[] array = list.toArray(new String[0]);

// Array to List
List<String> list = Arrays.asList(array);

// Set to List
List<String> list = new ArrayList<>(set);
```

## Iterator
```java
List<String> list = Arrays.asList("A", "B", "C");
Iterator<String> iterator = list.iterator();

while (iterator.hasNext()) {
    String element = iterator.next();
    if (element.equals("B")) {
        iterator.remove(); // Remove during iteration
    }
}
```

## Collections Utility Class
```java
import java.util.Collections;

List<Integer> list = Arrays.asList(5, 2, 8, 1, 9);

// Sort
Collections.sort(list);

// Reverse
Collections.reverse(list);

// Shuffle
Collections.shuffle(list);

// Find max/min
int max = Collections.max(list);
int min = Collections.min(list);

// Binary search (must be sorted)
Collections.sort(list);
int index = Collections.binarySearch(list, 5);
```

## Comparator and Comparable

### Comparable
```java
class Student implements Comparable<Student> {
    private String name;
    private int age;
    
    @Override
    public int compareTo(Student other) {
        return this.name.compareTo(other.name);
    }
}
```

### Comparator
```java
Comparator<Student> ageComparator = new Comparator<Student>() {
    @Override
    public int compare(Student s1, Student s2) {
        return Integer.compare(s1.getAge(), s2.getAge());
    }
};

// Using lambda (Java 8+)
Collections.sort(students, (s1, s2) -> s1.getAge() - s2.getAge());
```

## Complete Example
```java
import java.util.*;

public class CollectionsDemo {
    public static void main(String[] args) {
        // List example
        List<String> fruits = new ArrayList<>();
        fruits.add("Apple");
        fruits.add("Banana");
        fruits.add("Cherry");
        System.out.println("List: " + fruits);
        
        // Set example
        Set<Integer> numbers = new HashSet<>();
        numbers.add(1);
        numbers.add(2);
        numbers.add(1); // Duplicate
        System.out.println("Set: " + numbers);
        
        // Map example
        Map<String, Integer> scores = new HashMap<>();
        scores.put("Alice", 95);
        scores.put("Bob", 87);
        scores.put("Charlie", 92);
        System.out.println("Map: " + scores);
        
        // Queue example
        Queue<String> queue = new LinkedList<>();
        queue.offer("First");
        queue.offer("Second");
        queue.offer("Third");
        System.out.println("Queue: " + queue.poll());
    }
}
```

## Best Practices
1. Use ArrayList for frequent access
2. Use LinkedList for frequent insertions/deletions
3. Use HashSet for unique elements, no order needed
4. Use TreeSet for sorted unique elements
5. Use HashMap for key-value pairs, no order needed
6. Use TreeMap for sorted key-value pairs
7. Prefer interfaces (List, Set, Map) over concrete classes
8. Use generics for type safety


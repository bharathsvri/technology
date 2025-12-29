# Hash Tables (Java Implementation)

## Definition
A hash table (hash map) is a data structure that implements an associative array, mapping keys to values. It uses a hash function to compute an index into an array of buckets, from which the desired value can be found.

## Characteristics
- **Key-Value Pairs**: Stores data as (key, value) pairs
- **Hash Function**: Maps keys to array indices
- **Fast Lookup**: Average O(1) time complexity
- **Dynamic Size**: Can grow/shrink as needed
- **Unique Keys**: Typically, each key appears once

## How Hash Tables Work

### Basic Process
1. **Hash Function**: Converts key to hash code
2. **Compression**: Maps hash code to array index
3. **Storage**: Store value at computed index
4. **Collision Handling**: Manage multiple keys mapping to same index

## Hash Function

### Properties of Good Hash Function
- **Deterministic**: Same key → same hash value
- **Uniform Distribution**: Keys distributed evenly
- **Fast Computation**: O(1) time complexity
- **Minimize Collisions**: Different keys → different indices (when possible)

### Common Hash Functions
- **Division Method**: `h(k) = k % m` (m = table size)
- **Multiplication Method**: `h(k) = floor(m * (k * A mod 1))`
- **Universal Hashing**: Random hash function family

## Collision Resolution

### 1. Chaining (Separate Chaining)
- Each bucket contains a linked list
- Colliding keys stored in the same bucket's list
- Simple to implement
- **Load Factor**: α = n/m (n = entries, m = buckets)

### 2. Open Addressing
- All entries stored in the table itself
- Probe sequence when collision occurs
- Methods:
  - **Linear Probing**: Check next slot
  - **Quadratic Probing**: Check i²-th slot
  - **Double Hashing**: Use second hash function

## Time Complexity

| Operation | Average Case | Worst Case |
|-----------|--------------|------------|
| Insert | O(1) | O(n) |
| Search | O(1) | O(n) |
| Delete | O(1) | O(n) |
| Update | O(1) | O(n) |

**Note**: Worst case occurs with many collisions or poor hash function

## Space Complexity
- O(n) where n is the number of key-value pairs
- Additional space for collision handling

## Load Factor
- **Definition**: α = n/m (entries/buckets)
- **Optimal Range**: 0.5 to 0.75
- **High Load Factor**: More collisions, slower performance
- **Low Load Factor**: Wasted space
- **Rehashing**: Resize table when load factor exceeds threshold

## Advantages
- Very fast average-case performance (O(1))
- Flexible key types (strings, integers, objects)
- Efficient for large datasets
- Simple API (put, get, remove)
- Widely supported in programming languages

## Disadvantages
- Worst case O(n) performance
- No ordered traversal (keys not sorted)
- Requires good hash function
- Memory overhead
- Collision handling complexity

## Java Implementation

### HashMap with Chaining
```java
import java.util.*;

class HashEntry {
    int key;
    int value;
    
    HashEntry(int key, int value) {
        this.key = key;
        this.value = value;
    }
}

public class HashMap {
    private List<List<HashEntry>> buckets;
    private int capacity;
    private int size;
    private static final double LOAD_FACTOR = 0.75;
    
    public HashMap(int capacity) {
        this.capacity = capacity;
        this.size = 0;
        buckets = new ArrayList<>(capacity);
        for (int i = 0; i < capacity; i++) {
            buckets.add(new LinkedList<>());
        }
    }
    
    public HashMap() {
        this(16);  // Default capacity
    }
    
    private int hash(int key) {
        return Math.abs(key) % capacity;
    }
    
    public void put(int key, int value) {
        int index = hash(key);
        List<HashEntry> bucket = buckets.get(index);
        
        // Check if key already exists
        for (HashEntry entry : bucket) {
            if (entry.key == key) {
                entry.value = value;
                return;
            }
        }
        
        // Add new entry
        bucket.add(new HashEntry(key, value));
        size++;
        
        // Check load factor and rehash if needed
        if ((double) size / capacity > LOAD_FACTOR) {
            rehash();
        }
    }
    
    public Integer get(int key) {
        int index = hash(key);
        List<HashEntry> bucket = buckets.get(index);
        
        for (HashEntry entry : bucket) {
            if (entry.key == key) {
                return entry.value;
            }
        }
        
        return null;  // Key not found
    }
    
    public boolean containsKey(int key) {
        return get(key) != null;
    }
    
    public void remove(int key) {
        int index = hash(key);
        List<HashEntry> bucket = buckets.get(index);
        
        Iterator<HashEntry> iterator = bucket.iterator();
        while (iterator.hasNext()) {
            HashEntry entry = iterator.next();
            if (entry.key == key) {
                iterator.remove();
                size--;
                return;
            }
        }
    }
    
    private void rehash() {
        List<List<HashEntry>> oldBuckets = buckets;
        capacity *= 2;
        size = 0;
        buckets = new ArrayList<>(capacity);
        
        for (int i = 0; i < capacity; i++) {
            buckets.add(new LinkedList<>());
        }
        
        for (List<HashEntry> bucket : oldBuckets) {
            for (HashEntry entry : bucket) {
                put(entry.key, entry.value);
            }
        }
    }
    
    public int size() {
        return size;
    }
    
    public boolean isEmpty() {
        return size == 0;
    }
}
```

### Using Java HashMap
```java
import java.util.*;

public class HashMapExample {
    public static void main(String[] args) {
        // Create HashMap
        Map<String, Integer> map = new HashMap<>();
        
        // Insert
        map.put("apple", 5);
        map.put("banana", 3);
        map.put("orange", 8);
        
        // Get value
        Integer value = map.get("apple");  // Returns 5
        
        // Update
        map.put("apple", 10);
        
        // Check if key exists
        boolean exists = map.containsKey("banana");  // Returns true
        
        // Remove
        map.remove("banana");
        
        // Size
        int size = map.size();
        
        // Check if empty
        boolean empty = map.isEmpty();
        
        // Clear
        map.clear();
    }
}
```

## Example Applications

### Dictionary Implementation
```java
import java.util.*;

Map<String, Integer> dict = new HashMap<>();
dict.put("apple", 5);      // Insert
Integer value = dict.get("apple");  // Lookup O(1)
dict.put("apple", 10);     // Update
dict.remove("apple");      // Delete
```

### Frequency Counter
```java
import java.util.*;

// Count word frequencies
public static Map<String, Integer> countWordFrequencies(String[] words) {
    Map<String, Integer> freq = new HashMap<>();
    for (String word : words) {
        freq.put(word, freq.getOrDefault(word, 0) + 1);
    }
    return freq;
    // O(n) time, where n = number of words
}
```

### Caching (Memoization)
```java
import java.util.*;

// Memoization example
private Map<Integer, Long> cache = new HashMap<>();

public long fibonacci(int n) {
    if (cache.containsKey(n)) {
        return cache.get(n);  // O(1) lookup
    }
    
    long result;
    if (n <= 1) {
        result = n;
    } else {
        result = fibonacci(n - 1) + fibonacci(n - 2);
    }
    
    cache.put(n, result);
    return result;
}
```

### Two Sum Problem
```java
import java.util.*;

public static int[] twoSum(int[] nums, int target) {
    Map<Integer, Integer> map = new HashMap<>();
    
    for (int i = 0; i < nums.length; i++) {
        int complement = target - nums[i];
        if (map.containsKey(complement)) {
            return new int[]{map.get(complement), i};
        }
        map.put(nums[i], i);
    }
    
    return new int[0];  // No solution
}
```

### Group Anagrams
```java
import java.util.*;

public static List<List<String>> groupAnagrams(String[] strs) {
    Map<String, List<String>> map = new HashMap<>();
    
    for (String str : strs) {
        char[] chars = str.toCharArray();
        Arrays.sort(chars);
        String key = new String(chars);
        
        map.putIfAbsent(key, new ArrayList<>());
        map.get(key).add(str);
    }
    
    return new ArrayList<>(map.values());
}
```

## Variations

### 1. Hash Set
```java
import java.util.*;

Set<Integer> set = new HashSet<>();
set.add(1);
set.add(2);
set.add(3);
boolean contains = set.contains(2);  // true
set.remove(2);

// Remove duplicates from array
public static int[] removeDuplicates(int[] arr) {
    Set<Integer> unique = new HashSet<>();
    for (int num : arr) {
        unique.add(num);
    }
    return unique.stream().mapToInt(i -> i).toArray();
}
```

### 2. LinkedHashMap (Maintains Insertion Order)
```java
import java.util.*;

Map<String, Integer> linkedMap = new LinkedHashMap<>();
linkedMap.put("first", 1);
linkedMap.put("second", 2);
linkedMap.put("third", 3);
// Iteration order: first, second, third
```

### 3. TreeMap (Sorted Keys)
```java
import java.util.*;

Map<String, Integer> treeMap = new TreeMap<>();
treeMap.put("zebra", 1);
treeMap.put("apple", 2);
treeMap.put("banana", 3);
// Keys are sorted: apple, banana, zebra
```

## Common Problems
- Two Sum
- Group Anagrams
- Longest Substring Without Repeating Characters
- First Unique Character
- Contains Duplicate
- Design HashMap/HashSet
- LRU Cache (uses hash table + doubly linked list)
- Word Frequency Analysis

## Best Practices
- Choose appropriate initial size
- Monitor load factor
- Use built-in HashMap/HashSet when available
- Handle edge cases (null keys, empty table)
- Consider collision resolution method based on use case
- Test with realistic data distributions
- Use LinkedHashMap if insertion order matters
- Use TreeMap if sorted keys are needed

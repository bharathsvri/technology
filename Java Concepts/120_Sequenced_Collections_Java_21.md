# Sequenced Collections, Sequenced Sets, and Sequenced Maps (Java 21)

Java 21 introduces **encounter-order** interfaces so list/set/map types can expose **first/last** elements and **reversed** views in a uniform way.

## New Interfaces (Conceptual)
- **`SequencedCollection<E>`**: extends `Collection`; adds `getFirst()`, `getLast()`, `removeFirst()`, `removeLast()`, `reversed()`.
- **`SequencedSet<E>`**: ordered set without duplicates; `reversed()` returns another `SequencedSet`.
- **`SequencedMap<K,V>`**: ordered map; `firstEntry`, `lastEntry`, `pollFirstEntry`, `pollLastEntry`, `reversed()`.

## JDK Implementations
- **`ArrayList`**, **`LinkedList`**, **`LinkedHashSet`**, **`LinkedHashMap`**, **`TreeSet`**, **`TreeMap`** implement the appropriate sequenced interfaces.

## Example Ideas

```java
ArrayList<String> items = new ArrayList<>();
items.add("a");
items.add("b");
String first = items.getFirst();
SequencedCollection<String> rev = items.reversed(); // view
```

## Why It Matters
- Removes ad-hoc patterns like `list.get(0)` / `list.get(list.size()-1)` scattered in code.
- Makes **deque-like** operations discoverable on standard collections.

## Related Topics
- `17_Collections_Framework.md`
- `104_Java_Versions_New_Features.md`

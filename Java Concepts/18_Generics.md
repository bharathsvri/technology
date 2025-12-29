# Generics

## What are Generics?
Generics enable types (classes and interfaces) to be parameters when defining classes, interfaces, and methods. They provide type safety and eliminate the need for casting.

## Why Use Generics?
1. **Type Safety**: Catch errors at compile time
2. **Eliminate Casting**: No need for explicit type casting
3. **Code Reusability**: Write code once, use with different types

## Generic Classes

### Basic Syntax
```java
class ClassName<T> {
    private T data;
    
    public void setData(T data) {
        this.data = data;
    }
    
    public T getData() {
        return data;
    }
}
```

### Example
```java
class Box<T> {
    private T item;
    
    public void setItem(T item) {
        this.item = item;
    }
    
    public T getItem() {
        return item;
    }
}

// Usage
Box<String> stringBox = new Box<>();
stringBox.setItem("Hello");
String value = stringBox.getItem(); // No casting needed

Box<Integer> intBox = new Box<>();
intBox.setItem(100);
Integer number = intBox.getItem();
```

## Multiple Type Parameters
```java
class Pair<K, V> {
    private K key;
    private V value;
    
    public Pair(K key, V value) {
        this.key = key;
        this.value = value;
    }
    
    public K getKey() {
        return key;
    }
    
    public V getValue() {
        return value;
    }
}

// Usage
Pair<String, Integer> pair = new Pair<>("Age", 25);
String key = pair.getKey();
Integer value = pair.getValue();
```

## Generic Methods
```java
class Utils {
    // Generic method
    public static <T> void printArray(T[] array) {
        for (T element : array) {
            System.out.println(element);
        }
    }
    
    // Generic method with return type
    public static <T> T getFirst(T[] array) {
        return array[0];
    }
}

// Usage
String[] strings = {"A", "B", "C"};
Integer[] numbers = {1, 2, 3};

Utils.printArray(strings);
Utils.printArray(numbers);

String first = Utils.getFirst(strings);
```

## Bounded Type Parameters

### Upper Bound
```java
class NumberBox<T extends Number> {
    private T number;
    
    public NumberBox(T number) {
        this.number = number;
    }
    
    public double getDoubleValue() {
        return number.doubleValue();
    }
}

// Usage
NumberBox<Integer> intBox = new NumberBox<>(10);
NumberBox<Double> doubleBox = new NumberBox<>(10.5);
// NumberBox<String> stringBox = new NumberBox<>("Hello"); // Error
```

### Multiple Bounds
```java
class Calculator<T extends Number & Comparable<T>> {
    public T max(T a, T b) {
        return a.compareTo(b) > 0 ? a : b;
    }
}
```

## Wildcards

### Unbounded Wildcard (?)
```java
public void printList(List<?> list) {
    for (Object item : list) {
        System.out.println(item);
    }
}
```

### Upper Bounded Wildcard (? extends)
```java
public double sum(List<? extends Number> numbers) {
    double sum = 0.0;
    for (Number num : numbers) {
        sum += num.doubleValue();
    }
    return sum;
}

// Can accept List<Integer>, List<Double>, etc.
List<Integer> ints = Arrays.asList(1, 2, 3);
double result = sum(ints);
```

### Lower Bounded Wildcard (? super)
```java
public void addNumbers(List<? super Integer> list) {
    list.add(1);
    list.add(2);
    list.add(3);
}

// Can accept List<Integer>, List<Number>, List<Object>
List<Number> numbers = new ArrayList<>();
addNumbers(numbers);
```

## Generic Interfaces
```java
interface Repository<T> {
    void save(T entity);
    T findById(int id);
    void delete(T entity);
}

class UserRepository implements Repository<User> {
    @Override
    public void save(User entity) {
        // Implementation
    }
    
    @Override
    public User findById(int id) {
        // Implementation
        return null;
    }
    
    @Override
    public void delete(User entity) {
        // Implementation
    }
}
```

## Generic Collections
```java
// Without generics (old way)
List list = new ArrayList();
list.add("Hello");
String str = (String) list.get(0); // Casting required

// With generics (type safe)
List<String> list = new ArrayList<>();
list.add("Hello");
String str = list.get(0); // No casting needed
```

## Type Erasure
Generics are removed at compile time (type erasure).

```java
// At compile time
List<String> stringList = new ArrayList<>();
List<Integer> intList = new ArrayList<>();

// At runtime (after type erasure)
List stringList = new ArrayList();
List intList = new ArrayList();
```

## Common Generic Patterns

### Generic Stack
```java
class Stack<T> {
    private List<T> items = new ArrayList<>();
    
    public void push(T item) {
        items.add(item);
    }
    
    public T pop() {
        if (items.isEmpty()) {
            throw new RuntimeException("Stack is empty");
        }
        return items.remove(items.size() - 1);
    }
    
    public boolean isEmpty() {
        return items.isEmpty();
    }
}
```

### Generic Pair
```java
class Pair<T, U> {
    private T first;
    private U second;
    
    public Pair(T first, U second) {
        this.first = first;
        this.second = second;
    }
    
    public T getFirst() {
        return first;
    }
    
    public U getSecond() {
        return second;
    }
}
```

## Complete Example
```java
// Generic class
class Container<T> {
    private T value;
    
    public Container(T value) {
        this.value = value;
    }
    
    public T getValue() {
        return value;
    }
    
    public void setValue(T value) {
        this.value = value;
    }
    
    // Generic method
    public <U> void printValueAndType(U otherValue) {
        System.out.println("Container value: " + value);
        System.out.println("Other value: " + otherValue);
    }
}

// Usage
public class GenericsDemo {
    public static void main(String[] args) {
        Container<String> stringContainer = new Container<>("Hello");
        Container<Integer> intContainer = new Container<>(100);
        
        System.out.println(stringContainer.getValue());
        System.out.println(intContainer.getValue());
        
        stringContainer.printValueAndType(42);
    }
}
```

## Best Practices
1. Use descriptive type parameter names (T, E, K, V)
2. Prefer wildcards when you don't need type information
3. Use bounded wildcards for maximum flexibility
4. Avoid raw types (use generics)
5. Don't use generics with primitive types (use wrapper classes)
6. Be aware of type erasure limitations

## Common Generic Type Parameters
- **T** - Type
- **E** - Element (collections)
- **K** - Key (maps)
- **V** - Value (maps)
- **N** - Number
- **S, U, V** - Additional types


# String Manipulation

## String Class
String is an immutable sequence of characters in Java.

## Creating Strings
```java
// String literal (stored in string pool)
String str1 = "Hello";

// Using new keyword (creates new object)
String str2 = new String("Hello");

// From character array
char[] chars = {'H', 'e', 'l', 'l', 'o'};
String str3 = new String(chars);
```

## String Immutability
Strings are immutable - once created, cannot be changed.

```java
String str = "Hello";
str = str + " World"; // Creates new string, doesn't modify original
```

## String Methods

### Length and Empty Check
```java
String str = "Hello";
int length = str.length(); // 5
boolean isEmpty = str.isEmpty(); // false
boolean isBlank = str.isBlank(); // false (Java 11+)
```

### Character Access
```java
String str = "Hello";
char ch = str.charAt(0); // 'H'
char[] chars = str.toCharArray();
```

### Comparison
```java
String str1 = "Hello";
String str2 = "Hello";
String str3 = new String("Hello");

// equals() - compares content
boolean equal1 = str1.equals(str2); // true
boolean equal2 = str1.equals(str3); // true

// == - compares references
boolean sameRef = str1 == str2; // true (string pool)
boolean sameRef2 = str1 == str3; // false (different objects)

// equalsIgnoreCase()
boolean equal = "Hello".equalsIgnoreCase("hello"); // true

// compareTo() - lexicographic comparison
int result = "apple".compareTo("banana"); // negative
int result2 = "banana".compareTo("apple"); // positive
```

### Substring
```java
String str = "Hello World";
String sub1 = str.substring(6); // "World"
String sub2 = str.substring(0, 5); // "Hello"
```

### Concatenation
```java
String str1 = "Hello";
String str2 = "World";

// Using + operator
String result1 = str1 + " " + str2; // "Hello World"

// Using concat()
String result2 = str1.concat(" ").concat(str2);

// Using StringBuilder (for multiple concatenations)
StringBuilder sb = new StringBuilder();
sb.append(str1).append(" ").append(str2);
String result3 = sb.toString();
```

### Searching
```java
String str = "Hello World";

// Contains
boolean contains = str.contains("World"); // true

// Index of
int index = str.indexOf("World"); // 6
int lastIndex = str.lastIndexOf("l"); // 9
int indexFrom = str.indexOf("l", 3); // 3

// Starts with / Ends with
boolean starts = str.startsWith("Hello"); // true
boolean ends = str.endsWith("World"); // true
```

### Replacement
```java
String str = "Hello World";

// Replace character
String replaced = str.replace('l', 'L'); // "HeLLo WorLd"

// Replace string
String replaced2 = str.replace("World", "Java"); // "Hello Java"

// Replace all (regex)
String replaced3 = str.replaceAll("l", "L"); // "HeLLo WorLd"

// Replace first
String replaced4 = str.replaceFirst("l", "L"); // "HeLLo World"
```

### Case Conversion
```java
String str = "Hello World";

String upper = str.toUpperCase(); // "HELLO WORLD"
String lower = str.toLowerCase(); // "hello world"
```

### Trimming
```java
String str = "  Hello World  ";

String trimmed = str.trim(); // "Hello World"
String strip = str.strip(); // "Hello World" (Java 11+)
String stripLeading = str.stripLeading(); // "Hello World  "
String stripTrailing = str.stripTrailing(); // "  Hello World"
```

### Splitting
```java
String str = "apple,banana,cherry";

String[] fruits = str.split(",");
// ["apple", "banana", "cherry"]

String[] parts = str.split(",", 2);
// ["apple", "banana,cherry"]
```

### Joining (Java 8+)
```java
String[] fruits = {"apple", "banana", "cherry"};
String joined = String.join(", ", fruits);
// "apple, banana, cherry"
```

## StringBuilder
Mutable sequence of characters for efficient string building.

```java
StringBuilder sb = new StringBuilder();

// Append
sb.append("Hello");
sb.append(" ");
sb.append("World");

String result = sb.toString(); // "Hello World"

// Insert
sb.insert(5, " Java"); // "Hello Java World"

// Delete
sb.delete(5, 10); // "Hello World"

// Reverse
sb.reverse(); // "dlroW olleH"

// Capacity
int capacity = sb.capacity();
sb.ensureCapacity(50);
```

## StringBuffer
Thread-safe version of StringBuilder (synchronized).

```java
StringBuffer sb = new StringBuffer();
sb.append("Hello");
sb.append(" World");
String result = sb.toString();
```

## String Formatting

### format() Method
```java
String name = "John";
int age = 25;
double salary = 50000.5;

String formatted = String.format("Name: %s, Age: %d, Salary: %.2f", 
                                  name, age, salary);
// "Name: John, Age: 25, Salary: 50000.50"
```

### Format Specifiers
- `%s` - String
- `%d` - Integer
- `%f` - Float/Double
- `%c` - Character
- `%b` - Boolean
- `%n` - Newline

## Regular Expressions
```java
String text = "Hello123World456";

// Matches
boolean matches = text.matches(".*\\d+.*"); // true

// Replace all digits
String replaced = text.replaceAll("\\d+", "");
// "HelloWorld"

// Split by digits
String[] parts = text.split("\\d+");
// ["Hello", "World", ""]
```

## String Pool
Java maintains a pool of string literals for memory efficiency.

```java
String str1 = "Hello"; // In string pool
String str2 = "Hello"; // Reuses from pool
String str3 = new String("Hello"); // New object

System.out.println(str1 == str2); // true (same reference)
System.out.println(str1 == str3); // false (different objects)
System.out.println(str1.equals(str3)); // true (same content)
```

## Complete Example
```java
public class StringDemo {
    public static void main(String[] args) {
        // Creating strings
        String str1 = "Hello";
        String str2 = new String("World");
        
        // Concatenation
        String combined = str1 + " " + str2;
        System.out.println(combined); // "Hello World"
        
        // Methods
        System.out.println("Length: " + combined.length());
        System.out.println("Uppercase: " + combined.toUpperCase());
        System.out.println("Contains 'World': " + combined.contains("World"));
        
        // Substring
        String sub = combined.substring(0, 5);
        System.out.println("Substring: " + sub);
        
        // Split
        String[] words = combined.split(" ");
        for (String word : words) {
            System.out.println(word);
        }
        
        // StringBuilder
        StringBuilder sb = new StringBuilder();
        sb.append("Java");
        sb.append(" is");
        sb.append(" awesome");
        System.out.println(sb.toString());
    }
}
```

## Best Practices
1. Use string literals when possible (string pool)
2. Use StringBuilder for multiple concatenations
3. Use equals() for content comparison, not ==
4. Be careful with string immutability in loops
5. Use String.join() for joining collections
6. Prefer StringBuilder over StringBuffer unless thread-safety needed


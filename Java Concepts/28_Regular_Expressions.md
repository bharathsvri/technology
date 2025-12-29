# Regular Expressions

## What are Regular Expressions?
Regular expressions (regex) are patterns used to match character combinations in strings. Java provides the `java.util.regex` package for regex operations.

## Pattern and Matcher Classes

### Basic Usage
```java
import java.util.regex.Pattern;
import java.util.regex.Matcher;

String text = "Hello World";
String pattern = "World";

Pattern p = Pattern.compile(pattern);
Matcher m = p.matcher(text);

boolean found = m.find(); // true
```

## Common Patterns

### Literal Characters
```java
Pattern pattern = Pattern.compile("hello");
// Matches exactly "hello"
```

### Character Classes
```java
Pattern.compile("[abc]");        // Matches a, b, or c
Pattern.compile("[a-z]");       // Matches any lowercase letter
Pattern.compile("[A-Z]");       // Matches any uppercase letter
Pattern.compile("[0-9]");       // Matches any digit
Pattern.compile("[a-zA-Z]");    // Matches any letter
Pattern.compile("[^abc]");       // Matches anything except a, b, c
```

### Predefined Character Classes
```java
Pattern.compile("\\d");         // Digit [0-9]
Pattern.compile("\\D");         // Non-digit [^0-9]
Pattern.compile("\\s");         // Whitespace
Pattern.compile("\\S");         // Non-whitespace
Pattern.compile("\\w");         // Word character [a-zA-Z0-9_]
Pattern.compile("\\W");         // Non-word character
Pattern.compile(".");           // Any character except newline
```

### Quantifiers
```java
Pattern.compile("a?");           // Zero or one 'a'
Pattern.compile("a*");           // Zero or more 'a'
Pattern.compile("a+");           // One or more 'a'
Pattern.compile("a{3}");         // Exactly 3 'a'
Pattern.compile("a{3,}");        // 3 or more 'a'
Pattern.compile("a{3,5}");       // 3 to 5 'a'
```

### Anchors
```java
Pattern.compile("^hello");      // Starts with "hello"
Pattern.compile("world$");      // Ends with "world"
Pattern.compile("^hello world$"); // Exact match
```

### Groups
```java
Pattern pattern = Pattern.compile("(\\d{3})-(\\d{3})-(\\d{4})");
Matcher matcher = pattern.matcher("123-456-7890");
if (matcher.matches()) {
    String areaCode = matcher.group(1); // "123"
    String exchange = matcher.group(2);  // "456"
    String number = matcher.group(3);    // "7890"
}
```

## Common Methods

### matches()
```java
String text = "123-456-7890";
boolean matches = text.matches("\\d{3}-\\d{3}-\\d{4}"); // true
```

### find()
```java
Pattern pattern = Pattern.compile("\\d+");
Matcher matcher = pattern.matcher("abc123def456");
while (matcher.find()) {
    System.out.println(matcher.group()); // "123", then "456"
}
```

### replaceAll()
```java
String text = "Hello123World456";
String replaced = text.replaceAll("\\d+", "");
System.out.println(replaced); // "HelloWorld"
```

### replaceFirst()
```java
String text = "Hello123World456";
String replaced = text.replaceFirst("\\d+", "");
System.out.println(replaced); // "HelloWorld456"
```

### split()
```java
String text = "apple,banana,cherry";
String[] fruits = text.split(",");
// ["apple", "banana", "cherry"]

String text2 = "apple  banana   cherry";
String[] fruits2 = text2.split("\\s+");
// ["apple", "banana", "cherry"]
```

## Practical Examples

### Email Validation
```java
String email = "user@example.com";
String pattern = "^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\\.[a-zA-Z]{2,}$";
boolean isValid = email.matches(pattern);
```

### Phone Number
```java
String phone = "123-456-7890";
String pattern = "^\\d{3}-\\d{3}-\\d{4}$";
boolean isValid = phone.matches(pattern);
```

### Password Validation
```java
// At least 8 characters, one uppercase, one lowercase, one digit
String pattern = "^(?=.*[a-z])(?=.*[A-Z])(?=.*\\d).{8,}$";
String password = "Password123";
boolean isValid = password.matches(pattern);
```

### Extract Numbers
```java
String text = "Price: $19.99, Tax: $2.50";
Pattern pattern = Pattern.compile("\\d+\\.\\d+");
Matcher matcher = pattern.matcher(text);
while (matcher.find()) {
    System.out.println(matcher.group()); // "19.99", "2.50"
}
```

### Extract URLs
```java
String text = "Visit https://example.com for more info";
Pattern pattern = Pattern.compile("https?://[\\w.-]+");
Matcher matcher = pattern.matcher(text);
while (matcher.find()) {
    System.out.println(matcher.group()); // "https://example.com"
}
```

## Flags

### Case Insensitive
```java
Pattern pattern = Pattern.compile("hello", Pattern.CASE_INSENSITIVE);
Matcher matcher = pattern.matcher("HELLO");
boolean matches = matcher.matches(); // true
```

### Multiline
```java
Pattern pattern = Pattern.compile("^hello", Pattern.MULTILINE);
String text = "hello\nworld\nhello";
Matcher matcher = pattern.matcher(text);
while (matcher.find()) {
    System.out.println(matcher.group()); // "hello" twice
}
```

### Dotall
```java
Pattern pattern = Pattern.compile("hello.world", Pattern.DOTALL);
// '.' matches newline
```

## Complete Example
```java
import java.util.regex.*;

public class RegexDemo {
    public static void main(String[] args) {
        // Email validation
        String email = "user@example.com";
        String emailPattern = "^[\\w.%+-]+@[\\w.-]+\\.[a-zA-Z]{2,}$";
        System.out.println("Email valid: " + email.matches(emailPattern));
        
        // Extract phone numbers
        String text = "Call 123-456-7890 or 987-654-3210";
        Pattern phonePattern = Pattern.compile("\\d{3}-\\d{3}-\\d{4}");
        Matcher matcher = phonePattern.matcher(text);
        while (matcher.find()) {
            System.out.println("Phone: " + matcher.group());
        }
        
        // Replace
        String text2 = "Hello123World456";
        String cleaned = text2.replaceAll("\\d", "");
        System.out.println("Cleaned: " + cleaned);
        
        // Split
        String text3 = "apple,banana,cherry";
        String[] fruits = text3.split(",");
        for (String fruit : fruits) {
            System.out.println("Fruit: " + fruit);
        }
        
        // Groups
        Pattern datePattern = Pattern.compile("(\\d{2})/(\\d{2})/(\\d{4})");
        Matcher dateMatcher = datePattern.matcher("01/15/2024");
        if (dateMatcher.matches()) {
            System.out.println("Month: " + dateMatcher.group(1));
            System.out.println("Day: " + dateMatcher.group(2));
            System.out.println("Year: " + dateMatcher.group(3));
        }
    }
}
```

## Common Patterns Reference

| Pattern | Description |
|---------|-------------|
| `\d` | Digit |
| `\D` | Non-digit |
| `\w` | Word character |
| `\W` | Non-word character |
| `\s` | Whitespace |
| `\S` | Non-whitespace |
| `.` | Any character |
| `^` | Start of line |
| `$` | End of line |
| `*` | Zero or more |
| `+` | One or more |
| `?` | Zero or one |
| `{n}` | Exactly n times |
| `{n,}` | n or more times |
| `{n,m}` | Between n and m times |
| `[abc]` | Any of a, b, c |
| `[^abc]` | Not a, b, or c |
| `(abc)` | Group |

## Best Practices
1. Use Pattern.compile() for repeated use
2. Escape special characters with \\
3. Use non-capturing groups (?:...) when you don't need the group
4. Be careful with greedy vs reluctant quantifiers
5. Test regex patterns thoroughly
6. Use regex for validation, not for parsing complex structures
7. Consider using libraries for complex patterns (email, URL, etc.)


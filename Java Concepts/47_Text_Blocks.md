# Text Blocks (Java 13+)

## What are Text Blocks?
Text blocks are multi-line string literals that make it easier to work with strings spanning multiple lines.

## Basic Syntax
```java
// Old way (concatenation)
String html = "<html>\n" +
              "  <body>\n" +
              "    <p>Hello World</p>\n" +
              "  </body>\n" +
              "</html>";

// New way (text block)
String html = """
    <html>
      <body>
        <p>Hello World</p>
      </body>
    </html>
    """;
```

## Text Block Structure
```java
String text = """
    First line
    Second line
    Third line
    """;
```

## Preserving Formatting
```java
String json = """
    {
      "name": "John",
      "age": 25,
      "city": "NYC"
    }
    """;
```

## Escape Sequences
```java
// Newline
String text = """
    Line 1
    Line 2
    """;

// Escape quotes
String text = """
    He said "Hello"
    """;

// Escape backslash
String path = """
    C:\\Users\\Documents
    """;
```

## Indentation
```java
public void method() {
    String text = """
        This line has 8 spaces of indentation
        This line also has 8 spaces
        """;
    // The closing """ determines the left margin
}
```

## Interpolation (Java 21+)
```java
String name = "John";
int age = 25;

// String templates (Java 21+)
String message = STR."""
    Name: \{name}
    Age: \{age}
    """;
```

## Common Use Cases

### SQL Queries
```java
String query = """
    SELECT id, name, email
    FROM users
    WHERE age > ?
    ORDER BY name
    """;
```

### JSON
```java
String json = """
    {
      "id": 1,
      "name": "John",
      "email": "john@example.com"
    }
    """;
```

### HTML
```java
String html = """
    <div class="container">
      <h1>Welcome</h1>
      <p>This is a paragraph</p>
    </div>
    """;
```

### XML
```java
String xml = """
    <?xml version="1.0"?>
    <person>
      <name>John</name>
      <age>25</age>
    </person>
    """;
```

## Methods on Text Blocks
```java
String text = """
    Hello
    World
    """;

text.strip();        // Remove leading/trailing whitespace
text.stripIndent();  // Remove common indentation
text.translateEscapes(); // Process escape sequences
```

## Best Practices
1. Use text blocks for multi-line strings
2. Align closing """ with content for proper indentation
3. Use escape sequences when needed
4. Consider readability
5. Use for SQL, JSON, HTML, XML
6. Be aware of trailing newline
7. Use stripIndent() if needed

## Complete Example
```java
public class TextBlockDemo {
    public static void main(String[] args) {
        // HTML
        String html = """
            <html>
              <head>
                <title>My Page</title>
              </head>
              <body>
                <h1>Welcome</h1>
              </body>
            </html>
            """;
        
        // JSON
        String json = """
            {
              "users": [
                {
                  "id": 1,
                  "name": "John"
                },
                {
                  "id": 2,
                  "name": "Jane"
                }
              ]
            }
            """;
        
        // SQL
        String sql = """
            SELECT u.id, u.name, u.email
            FROM users u
            WHERE u.active = true
            ORDER BY u.name
            """;
        
        System.out.println(html);
        System.out.println(json);
        System.out.println(sql);
    }
}
```


# Packages

## What are Packages?
Packages are used to organize classes and interfaces into namespaces. They help avoid naming conflicts and provide access control.

## Package Declaration
```java
package com.example.util;

public class StringUtils {
    // Class code
}
```

## Package Naming Convention
- Use lowercase letters
- Use reverse domain name (com.company.package)
- Use meaningful names
- Avoid Java keywords

Examples:
- `com.example.util`
- `org.apache.commons`
- `java.util`

## Importing Packages

### Single Import
```java
import java.util.ArrayList;
import java.util.List;
```

### Wildcard Import
```java
import java.util.*;
```

### Static Import
```java
import static java.lang.Math.PI;
import static java.lang.Math.sqrt;

// Usage
double area = PI * radius * radius;
double result = sqrt(16);
```

### Fully Qualified Name
```java
java.util.ArrayList<String> list = new java.util.ArrayList<>();
```

## Access Modifiers with Packages

### Package-Private (Default)
```java
// In package com.example
class PackagePrivate {
    // Accessible within same package
}
```

### Public
```java
// In package com.example
public class PublicClass {
    // Accessible from any package
}
```

### Protected
```java
// In package com.example
public class Parent {
    protected int field; // Accessible in package and subclasses
}
```

## Creating Packages

### Directory Structure
```
src/
  com/
    example/
      util/
        StringUtils.java
      model/
        User.java
```

### Compiling Packages
```bash
javac -d . com/example/util/StringUtils.java
```

### Running with Packages
```bash
java com.example.util.StringUtils
```

## Complete Example
```java
// File: com/example/util/StringUtils.java
package com.example.util;

public class StringUtils {
    public static String reverse(String str) {
        return new StringBuilder(str).reverse().toString();
    }
    
    public static boolean isEmpty(String str) {
        return str == null || str.isEmpty();
    }
}

// File: com/example/Main.java
package com.example;

import com.example.util.StringUtils;

public class Main {
    public static void main(String[] args) {
        String reversed = StringUtils.reverse("Hello");
        System.out.println(reversed);
        
        boolean empty = StringUtils.isEmpty("");
        System.out.println(empty);
    }
}
```

## Common Java Packages

### java.lang
Automatically imported. Contains fundamental classes.
- `String`, `Object`, `Integer`, `System`, etc.

### java.util
Utility classes and collections.
- `ArrayList`, `HashMap`, `Date`, `Scanner`, etc.

### java.io
Input/output operations.
- `File`, `InputStream`, `OutputStream`, etc.

### java.net
Networking classes.
- `URL`, `Socket`, `ServerSocket`, etc.

### java.sql
Database connectivity.
- `Connection`, `Statement`, `ResultSet`, etc.

## Best Practices
1. Use meaningful package names
2. Follow reverse domain naming convention
3. Keep packages focused (single responsibility)
4. Avoid deep package hierarchies
5. Use lowercase for package names
6. Import only what you need
7. Avoid wildcard imports in production code
8. Organize related classes in same package


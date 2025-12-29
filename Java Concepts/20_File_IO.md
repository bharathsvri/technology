# File I/O

## File Operations

### File Class
Represents file and directory pathnames.

```java
import java.io.File;

// Create File object
File file = new File("example.txt");
File dir = new File("C:/Users/Documents");

// Check existence
boolean exists = file.exists();

// Check if file or directory
boolean isFile = file.isFile();
boolean isDir = file.isDirectory();

// Get information
String name = file.getName();
String path = file.getPath();
String absolutePath = file.getAbsolutePath();
long size = file.length();

// Create directory
File newDir = new File("newFolder");
newDir.mkdir(); // Create single directory
newDir.mkdirs(); // Create directory and parents

// Delete
file.delete();

// List files
File[] files = dir.listFiles();
```

## Reading Files

### FileReader (Character Stream)
```java
import java.io.FileReader;
import java.io.IOException;

try (FileReader reader = new FileReader("file.txt")) {
    int character;
    while ((character = reader.read()) != -1) {
        System.out.print((char) character);
    }
} catch (IOException e) {
    e.printStackTrace();
}
```

### BufferedReader
Efficient reading with buffering.

```java
import java.io.BufferedReader;
import java.io.FileReader;
import java.io.IOException;

try (BufferedReader reader = new BufferedReader(
        new FileReader("file.txt"))) {
    String line;
    while ((line = reader.readLine()) != null) {
        System.out.println(line);
    }
} catch (IOException e) {
    e.printStackTrace();
}
```

### Scanner
Simple text file reading.

```java
import java.io.File;
import java.io.FileNotFoundException;
import java.util.Scanner;

try (Scanner scanner = new Scanner(new File("file.txt"))) {
    while (scanner.hasNextLine()) {
        String line = scanner.nextLine();
        System.out.println(line);
    }
} catch (FileNotFoundException e) {
    e.printStackTrace();
}
```

### Files Class (Java 7+)
Modern way to read files.

```java
import java.nio.file.Files;
import java.nio.file.Paths;
import java.io.IOException;
import java.util.List;

// Read all lines
try {
    List<String> lines = Files.readAllLines(Paths.get("file.txt"));
    for (String line : lines) {
        System.out.println(line);
    }
} catch (IOException e) {
    e.printStackTrace();
}

// Read as string
try {
    String content = Files.readString(Paths.get("file.txt"));
    System.out.println(content);
} catch (IOException e) {
    e.printStackTrace();
}
```

## Writing Files

### FileWriter (Character Stream)
```java
import java.io.FileWriter;
import java.io.IOException;

try (FileWriter writer = new FileWriter("output.txt")) {
    writer.write("Hello World\n");
    writer.write("This is a test");
} catch (IOException e) {
    e.printStackTrace();
}

// Append mode
try (FileWriter writer = new FileWriter("output.txt", true)) {
    writer.write("\nAppended text");
} catch (IOException e) {
    e.printStackTrace();
}
```

### BufferedWriter
Efficient writing with buffering.

```java
import java.io.BufferedWriter;
import java.io.FileWriter;
import java.io.IOException;

try (BufferedWriter writer = new BufferedWriter(
        new FileWriter("output.txt"))) {
    writer.write("Line 1");
    writer.newLine();
    writer.write("Line 2");
} catch (IOException e) {
    e.printStackTrace();
}
```

### PrintWriter
Formatted output to file.

```java
import java.io.PrintWriter;
import java.io.FileNotFoundException;

try (PrintWriter writer = new PrintWriter("output.txt")) {
    writer.println("Hello");
    writer.printf("Number: %d\n", 42);
    writer.printf("Price: %.2f\n", 19.99);
} catch (FileNotFoundException e) {
    e.printStackTrace();
}
```

### Files Class (Java 7+)
```java
import java.nio.file.Files;
import java.nio.file.Paths;
import java.io.IOException;
import java.util.Arrays;
import java.util.List;

// Write lines
try {
    List<String> lines = Arrays.asList("Line 1", "Line 2", "Line 3");
    Files.write(Paths.get("output.txt"), lines);
} catch (IOException e) {
    e.printStackTrace();
}

// Write string
try {
    String content = "Hello World";
    Files.writeString(Paths.get("output.txt"), content);
} catch (IOException e) {
    e.printStackTrace();
}
```

## Binary File I/O

### FileInputStream
Reading binary files.

```java
import java.io.FileInputStream;
import java.io.IOException;

try (FileInputStream fis = new FileInputStream("image.jpg")) {
    int byteData;
    while ((byteData = fis.read()) != -1) {
        // Process byte
    }
} catch (IOException e) {
    e.printStackTrace();
}
```

### FileOutputStream
Writing binary files.

```java
import java.io.FileOutputStream;
import java.io.IOException;

byte[] data = {1, 2, 3, 4, 5};

try (FileOutputStream fos = new FileOutputStream("output.bin")) {
    fos.write(data);
} catch (IOException e) {
    e.printStackTrace();
}
```

### BufferedInputStream/BufferedOutputStream
```java
import java.io.*;

try (BufferedInputStream bis = new BufferedInputStream(
        new FileInputStream("input.bin"));
     BufferedOutputStream bos = new BufferedOutputStream(
        new FileOutputStream("output.bin"))) {
    
    int byteData;
    while ((byteData = bis.read()) != -1) {
        bos.write(byteData);
    }
} catch (IOException e) {
    e.printStackTrace();
}
```

## Object Serialization

### Serializable Interface
```java
import java.io.*;

class Person implements Serializable {
    private String name;
    private int age;
    
    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }
    
    // Getters and setters
}

// Writing object
try (ObjectOutputStream oos = new ObjectOutputStream(
        new FileOutputStream("person.ser"))) {
    Person person = new Person("John", 25);
    oos.writeObject(person);
} catch (IOException e) {
    e.printStackTrace();
}

// Reading object
try (ObjectInputStream ois = new ObjectInputStream(
        new FileInputStream("person.ser"))) {
    Person person = (Person) ois.readObject();
    System.out.println(person.getName());
} catch (IOException | ClassNotFoundException e) {
    e.printStackTrace();
}
```

## NIO (New I/O) - Java 7+

### Path and Paths
```java
import java.nio.file.Path;
import java.nio.file.Paths;

Path path = Paths.get("file.txt");
Path absolutePath = Paths.get("C:/Users/Documents/file.txt");
Path relativePath = Paths.get("folder", "subfolder", "file.txt");
```

### Files Utility Methods
```java
import java.nio.file.Files;
import java.nio.file.Path;
import java.nio.file.Paths;
import java.nio.file.StandardCopyOption;

Path source = Paths.get("source.txt");
Path target = Paths.get("target.txt");

// Copy file
Files.copy(source, target, StandardCopyOption.REPLACE_EXISTING);

// Move file
Files.move(source, target, StandardCopyOption.REPLACE_EXISTING);

// Delete file
Files.delete(target);

// Check existence
boolean exists = Files.exists(target);

// Get file size
long size = Files.size(target);
```

## Complete Example
```java
import java.io.*;
import java.nio.file.Files;
import java.nio.file.Paths;
import java.util.List;

public class FileIODemo {
    public static void main(String[] args) {
        // Write file
        try (BufferedWriter writer = Files.newBufferedWriter(
                Paths.get("demo.txt"))) {
            writer.write("Line 1\n");
            writer.write("Line 2\n");
            writer.write("Line 3");
        } catch (IOException e) {
            e.printStackTrace();
        }
        
        // Read file
        try {
            List<String> lines = Files.readAllLines(Paths.get("demo.txt"));
            for (String line : lines) {
                System.out.println(line);
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
        
        // File operations
        File file = new File("demo.txt");
        System.out.println("Exists: " + file.exists());
        System.out.println("Size: " + file.length());
        System.out.println("Path: " + file.getAbsolutePath());
    }
}
```

## Best Practices
1. Always use try-with-resources for automatic resource management
2. Handle IOException properly
3. Use BufferedReader/BufferedWriter for text files
4. Use Files class for modern file operations (Java 7+)
5. Close streams explicitly if not using try-with-resources
6. Check file existence before operations
7. Use appropriate stream types (character vs byte)
8. Handle file permissions and security


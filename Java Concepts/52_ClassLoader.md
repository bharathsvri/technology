# ClassLoader

## What is ClassLoader?
ClassLoader is responsible for loading classes into the JVM. It follows a delegation model.

## ClassLoader Hierarchy
```
Bootstrap ClassLoader (null)
    ↓
Extension ClassLoader
    ↓
Application/System ClassLoader
    ↓
Custom ClassLoader
```

## Built-in ClassLoaders

### Bootstrap ClassLoader
Loads core Java classes (rt.jar).

```java
// Bootstrap classloader is represented as null
ClassLoader bootstrap = String.class.getClassLoader();
System.out.println(bootstrap); // null
```

### Extension ClassLoader
Loads extension classes.

```java
ClassLoader extension = ExtensionClassLoader.class.getClassLoader();
```

### Application ClassLoader
Loads application classes from classpath.

```java
ClassLoader appClassLoader = MyClass.class.getClassLoader();
System.out.println(appClassLoader); // AppClassLoader
```

## Getting ClassLoader
```java
// From class
ClassLoader loader = MyClass.class.getClassLoader();

// From thread
ClassLoader loader = Thread.currentThread().getContextClassLoader();

// System classloader
ClassLoader loader = ClassLoader.getSystemClassLoader();
```

## Custom ClassLoader

### Basic Custom ClassLoader
```java
import java.io.*;

class CustomClassLoader extends ClassLoader {
    private String classPath;
    
    public CustomClassLoader(String classPath) {
        this.classPath = classPath;
    }
    
    @Override
    protected Class<?> findClass(String name) throws ClassNotFoundException {
        byte[] classData = loadClassData(name);
        if (classData == null) {
            throw new ClassNotFoundException();
        }
        return defineClass(name, classData, 0, classData.length);
    }
    
    private byte[] loadClassData(String className) {
        String fileName = className.replace('.', File.separatorChar) + ".class";
        File file = new File(classPath, fileName);
        
        try (FileInputStream fis = new FileInputStream(file);
             ByteArrayOutputStream baos = new ByteArrayOutputStream()) {
            
            int bytesRead;
            byte[] buffer = new byte[1024];
            while ((bytesRead = fis.read(buffer)) != -1) {
                baos.write(buffer, 0, bytesRead);
            }
            return baos.toByteArray();
        } catch (IOException e) {
            return null;
        }
    }
}
```

## Loading Classes Dynamically
```java
// Using custom classloader
CustomClassLoader loader = new CustomClassLoader("/path/to/classes");
Class<?> clazz = loader.loadClass("com.example.MyClass");
Object instance = clazz.newInstance();
```

## ClassLoader Delegation Model
```java
class MyClassLoader extends ClassLoader {
    @Override
    protected Class<?> loadClass(String name, boolean resolve) 
            throws ClassNotFoundException {
        // First, check if class is already loaded
        Class<?> c = findLoadedClass(name);
        if (c == null) {
            try {
                // Delegate to parent
                if (getParent() != null) {
                    c = getParent().loadClass(name);
                } else {
                    c = findBootstrapClassOrNull(name);
                }
            } catch (ClassNotFoundException e) {
                // If not found, try to find it ourselves
                c = findClass(name);
            }
        }
        if (resolve) {
            resolveClass(c);
        }
        return c;
    }
}
```

## URLClassLoader
Load classes from URLs.

```java
import java.net.*;

try {
    URL[] urls = {
        new URL("file:/path/to/classes/"),
        new URL("file:/path/to/jar.jar")
    };
    
    URLClassLoader loader = new URLClassLoader(urls);
    Class<?> clazz = loader.loadClass("com.example.MyClass");
    loader.close();
} catch (Exception e) {
    e.printStackTrace();
}
```

## Complete Example
```java
import java.io.*;

public class ClassLoaderDemo {
    public static void main(String[] args) {
        // Get current classloader
        ClassLoader loader = ClassLoaderDemo.class.getClassLoader();
        System.out.println("Current classloader: " + loader);
        System.out.println("Parent: " + loader.getParent());
        
        // Load class
        try {
            Class<?> clazz = loader.loadClass("java.util.ArrayList");
            System.out.println("Loaded: " + clazz.getName());
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        }
        
        // Get resources
        java.io.InputStream is = loader.getResourceAsStream("config.properties");
        if (is != null) {
            System.out.println("Resource found");
        }
    }
}
```

## Best Practices
1. Understand delegation model
2. Use URLClassLoader for loading from URLs
3. Close ClassLoader when done (Java 7+)
4. Be careful with custom ClassLoaders
5. Don't break classloader hierarchy
6. Handle ClassNotFoundException properly
7. Use appropriate classloader for context


# Reflection

## What is Reflection?
Reflection is the ability of a program to examine and modify its own structure and behavior at runtime. It allows inspection of classes, interfaces, fields, and methods.

## Getting Class Information

### Getting Class Object
```java
// Method 1: Using .class
Class<?> clazz = String.class;

// Method 2: Using getClass()
String str = "Hello";
Class<?> clazz2 = str.getClass();

// Method 3: Using Class.forName()
Class<?> clazz3 = Class.forName("java.lang.String");
```

### Class Information
```java
Class<?> clazz = String.class;

String name = clazz.getName();           // "java.lang.String"
String simpleName = clazz.getSimpleName(); // "String"
Package pkg = clazz.getPackage();
Class<?> superclass = clazz.getSuperclass();
Class<?>[] interfaces = clazz.getInterfaces();
int modifiers = clazz.getModifiers();
```

## Inspecting Fields

### Getting Fields
```java
import java.lang.reflect.Field;

class Person {
    private String name;
    public int age;
    protected String city;
}

Class<?> clazz = Person.class;

// Get all public fields (including inherited)
Field[] publicFields = clazz.getFields();

// Get all declared fields (including private)
Field[] allFields = clazz.getDeclaredFields();

// Get specific field
try {
    Field nameField = clazz.getDeclaredField("name");
} catch (NoSuchFieldException e) {
    e.printStackTrace();
}
```

### Accessing Field Values
```java
class Person {
    public String name = "John";
    private int age = 25;
}

Person person = new Person();
Class<?> clazz = person.getClass();

try {
    // Access public field
    Field nameField = clazz.getField("name");
    String name = (String) nameField.get(person);
    System.out.println(name); // "John"
    
    // Access private field
    Field ageField = clazz.getDeclaredField("age");
    ageField.setAccessible(true); // Make accessible
    int age = (int) ageField.get(person);
    System.out.println(age); // 25
    
    // Modify field
    nameField.set(person, "Jane");
    ageField.set(person, 30);
    
} catch (Exception e) {
    e.printStackTrace();
}
```

## Inspecting Methods

### Getting Methods
```java
import java.lang.reflect.Method;

class Calculator {
    public int add(int a, int b) {
        return a + b;
    }
    
    private void display() {
        System.out.println("Calculator");
    }
}

Class<?> clazz = Calculator.class;

// Get all public methods
Method[] publicMethods = clazz.getMethods();

// Get all declared methods
Method[] allMethods = clazz.getDeclaredMethods();

// Get specific method
try {
    Method addMethod = clazz.getMethod("add", int.class, int.class);
} catch (NoSuchMethodException e) {
    e.printStackTrace();
}
```

### Invoking Methods
```java
Calculator calc = new Calculator();
Class<?> clazz = calc.getClass();

try {
    // Invoke public method
    Method addMethod = clazz.getMethod("add", int.class, int.class);
    int result = (int) addMethod.invoke(calc, 5, 3);
    System.out.println(result); // 8
    
    // Invoke private method
    Method displayMethod = clazz.getDeclaredMethod("display");
    displayMethod.setAccessible(true);
    displayMethod.invoke(calc);
    
} catch (Exception e) {
    e.printStackTrace();
}
```

## Inspecting Constructors

### Getting Constructors
```java
import java.lang.reflect.Constructor;

class Person {
    private String name;
    private int age;
    
    public Person() {
    }
    
    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }
}

Class<?> clazz = Person.class;

// Get all constructors
Constructor<?>[] constructors = clazz.getConstructors();

// Get specific constructor
try {
    Constructor<?> constructor = clazz.getConstructor(String.class, int.class);
} catch (NoSuchMethodException e) {
    e.printStackTrace();
}
```

### Creating Instances
```java
try {
    // Using default constructor
    Constructor<?> defaultConstructor = Person.class.getConstructor();
    Person person1 = (Person) defaultConstructor.newInstance();
    
    // Using parameterized constructor
    Constructor<?> paramConstructor = Person.class.getConstructor(
        String.class, int.class);
    Person person2 = (Person) paramConstructor.newInstance("John", 25);
    
    // Using Class.newInstance() (deprecated)
    Person person3 = (Person) Person.class.newInstance();
    
} catch (Exception e) {
    e.printStackTrace();
}
```

## Getting Modifiers
```java
import java.lang.reflect.Modifier;

Class<?> clazz = Person.class;
int modifiers = clazz.getModifiers();

System.out.println(Modifier.isPublic(modifiers));
System.out.println(Modifier.isPrivate(modifiers));
System.out.println(Modifier.isStatic(modifiers));
System.out.println(Modifier.isFinal(modifiers));
```

## Annotations
```java
import java.lang.annotation.Annotation;

Class<?> clazz = MyClass.class;

// Get all annotations
Annotation[] annotations = clazz.getAnnotations();

// Check for specific annotation
if (clazz.isAnnotationPresent(MyAnnotation.class)) {
    MyAnnotation annotation = clazz.getAnnotation(MyAnnotation.class);
}
```

## Complete Example
```java
import java.lang.reflect.*;

class Student {
    private String name;
    private int age;
    
    public Student() {
    }
    
    public Student(String name, int age) {
        this.name = name;
        this.age = age;
    }
    
    public void display() {
        System.out.println("Name: " + name + ", Age: " + age);
    }
    
    private void privateMethod() {
        System.out.println("Private method called");
    }
}

public class ReflectionDemo {
    public static void main(String[] args) {
        try {
            // Get class
            Class<?> clazz = Student.class;
            System.out.println("Class: " + clazz.getName());
            
            // Get fields
            Field[] fields = clazz.getDeclaredFields();
            System.out.println("\nFields:");
            for (Field field : fields) {
                System.out.println("  " + Modifier.toString(field.getModifiers()) + 
                                 " " + field.getType().getSimpleName() + 
                                 " " + field.getName());
            }
            
            // Get constructors
            Constructor<?>[] constructors = clazz.getConstructors();
            System.out.println("\nConstructors:");
            for (Constructor<?> constructor : constructors) {
                System.out.println("  " + constructor);
            }
            
            // Create instance
            Constructor<?> constructor = clazz.getConstructor(String.class, int.class);
            Student student = (Student) constructor.newInstance("John", 20);
            
            // Access private field
            Field nameField = clazz.getDeclaredField("name");
            nameField.setAccessible(true);
            nameField.set(student, "Jane");
            
            // Invoke method
            Method displayMethod = clazz.getMethod("display");
            displayMethod.invoke(student);
            
            // Invoke private method
            Method privateMethod = clazz.getDeclaredMethod("privateMethod");
            privateMethod.setAccessible(true);
            privateMethod.invoke(student);
            
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

## Use Cases
1. **Frameworks**: Spring, Hibernate use reflection for dependency injection
2. **Testing**: JUnit uses reflection to run test methods
3. **Serialization**: JSON/XML libraries use reflection
4. **IDE Tools**: Code completion, debugging
5. **Dynamic Proxies**: Creating proxy objects at runtime

## Best Practices
1. Use reflection sparingly (performance overhead)
2. Cache reflection results when possible
3. Handle exceptions properly
4. Be aware of security implications
5. Use setAccessible() carefully
6. Prefer normal API when possible
7. Document reflection usage clearly

## Security Considerations
```java
// Reflection can bypass access control
Field field = clazz.getDeclaredField("privateField");
field.setAccessible(true); // Bypasses private access
field.set(object, value);

// Security manager can restrict reflection
SecurityManager manager = System.getSecurityManager();
if (manager != null) {
    manager.checkPermission(new ReflectPermission("suppressAccessChecks"));
}
```


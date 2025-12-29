# Annotations

## What are Annotations?
Annotations provide metadata about program elements (classes, methods, fields, etc.). They don't change the execution of code but provide information to compilers, tools, and runtime.

## Built-in Annotations

### @Override
Indicates method overrides a superclass method.

```java
class Parent {
    void display() {
        System.out.println("Parent");
    }
}

class Child extends Parent {
    @Override
    void display() {
        System.out.println("Child");
    }
}
```

### @Deprecated
Marks element as deprecated (no longer recommended).

```java
@Deprecated
public void oldMethod() {
    // Old implementation
}

@Deprecated(since = "2.0", forRemoval = true)
public void anotherOldMethod() {
    // Will be removed in future version
}
```

### @SuppressWarnings
Suppresses compiler warnings.

```java
@SuppressWarnings("unchecked")
List list = new ArrayList();

@SuppressWarnings({"unchecked", "deprecation"})
void method() {
    // Code with warnings
}
```

### @SafeVarargs
Suppresses warnings for varargs methods.

```java
@SafeVarargs
public final <T> void printItems(T... items) {
    for (T item : items) {
        System.out.println(item);
    }
}
```

### @FunctionalInterface
Indicates interface is a functional interface.

```java
@FunctionalInterface
interface Calculator {
    int calculate(int a, int b);
}
```

## Creating Custom Annotations

### Basic Annotation
```java
@interface MyAnnotation {
    String value();
}

@MyAnnotation("Hello")
class MyClass {
}
```

### Annotation with Multiple Elements
```java
@interface Author {
    String name();
    String date();
    int version() default 1;
}

@Author(name = "John Doe", date = "2024-01-01", version = 2)
class MyClass {
}
```

### Single Element Annotation
```java
@interface SingleValue {
    String value();
}

@SingleValue("Hello")
class MyClass {
}
```

### Marker Annotation (No Elements)
```java
@interface Marker {
}

@Marker
class MyClass {
}
```

## Annotation Retention

### @Retention
Specifies how long annotation is retained.

```java
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;

@Retention(RetentionPolicy.SOURCE)  // Discarded by compiler
@interface SourceAnnotation {
}

@Retention(RetentionPolicy.CLASS)  // Available in class file
@interface ClassAnnotation {
}

@Retention(RetentionPolicy.RUNTIME) // Available at runtime
@interface RuntimeAnnotation {
}
```

## Annotation Target

### @Target
Specifies where annotation can be used.

```java
import java.lang.annotation.Target;
import java.lang.annotation.ElementType;

@Target(ElementType.METHOD)
@interface MethodOnly {
}

@Target({ElementType.METHOD, ElementType.FIELD})
@interface MethodAndField {
}

// ElementType values:
// TYPE, FIELD, METHOD, PARAMETER, CONSTRUCTOR,
// LOCAL_VARIABLE, ANNOTATION_TYPE, PACKAGE, TYPE_PARAMETER, TYPE_USE
```

## Complete Custom Annotation Example
```java
import java.lang.annotation.*;

@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.METHOD)
@interface Test {
    String description() default "";
    int priority() default 0;
    boolean enabled() default true;
}

class TestRunner {
    @Test(description = "Test addition", priority = 1)
    public void testAdd() {
        System.out.println("Running testAdd");
    }
    
    @Test(description = "Test subtraction", priority = 2, enabled = false)
    public void testSubtract() {
        System.out.println("Running testSubtract");
    }
}
```

## Runtime Annotation Processing
```java
import java.lang.reflect.Method;

@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.METHOD)
@interface Test {
    String value();
}

class TestClass {
    @Test("test1")
    public void method1() {
    }
    
    @Test("test2")
    public void method2() {
    }
}

// Process annotations at runtime
public class AnnotationProcessor {
    public static void main(String[] args) {
        Class<?> clazz = TestClass.class;
        Method[] methods = clazz.getDeclaredMethods();
        
        for (Method method : methods) {
            if (method.isAnnotationPresent(Test.class)) {
                Test test = method.getAnnotation(Test.class);
                System.out.println("Method: " + method.getName() + 
                                 ", Test: " + test.value());
            }
        }
    }
}
```

## Common Framework Annotations

### Spring Framework
```java
@Component
@Service
@Repository
@Controller
@Autowired
@RequestMapping
```

### JUnit
```java
@Test
@Before
@After
@BeforeClass
@AfterClass
@Ignore
```

### Hibernate/JPA
```java
@Entity
@Table
@Id
@Column
@OneToMany
@ManyToOne
```

## Annotation Inheritance
```java
@Inherited
@Retention(RetentionPolicy.RUNTIME)
@interface InheritedAnnotation {
}

@InheritedAnnotation
class Parent {
}

class Child extends Parent {
    // Inherits @InheritedAnnotation
}
```

## Repeatable Annotations (Java 8+)
```java
import java.lang.annotation.Repeatable;

@Repeatable(Schedules.class)
@interface Schedule {
    String day();
    int hour();
}

@interface Schedules {
    Schedule[] value();
}

@Schedule(day = "Monday", hour = 9)
@Schedule(day = "Wednesday", hour = 10)
class Task {
}
```

## Best Practices
1. Use meaningful annotation names
2. Provide default values when appropriate
3. Document annotation purpose and usage
4. Use appropriate retention policy
5. Specify target elements clearly
6. Keep annotations simple and focused
7. Use @Inherited when inheritance makes sense


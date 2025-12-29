# Inner Classes

## What are Inner Classes?
Inner classes are classes defined within another class. They provide better encapsulation and can access outer class members.

## Types of Inner Classes

### 1. Non-Static Inner Class (Member Inner Class)
```java
class Outer {
    private int outerField = 10;
    
    class Inner {
        void display() {
            System.out.println("Outer field: " + outerField);
        }
    }
}

// Usage
Outer outer = new Outer();
Outer.Inner inner = outer.new Inner();
inner.display();
```

### 2. Static Inner Class
```java
class Outer {
    private static int staticField = 20;
    
    static class StaticInner {
        void display() {
            System.out.println("Static field: " + staticField);
        }
    }
}

// Usage
Outer.StaticInner inner = new Outer.StaticInner();
inner.display();
```

### 3. Local Inner Class
```java
class Outer {
    void method() {
        class LocalInner {
            void display() {
                System.out.println("Local inner class");
            }
        }
        
        LocalInner local = new LocalInner();
        local.display();
    }
}
```

### 4. Anonymous Inner Class
```java
interface Greeting {
    void greet();
}

class Outer {
    void method() {
        Greeting greeting = new Greeting() {
            @Override
            public void greet() {
                System.out.println("Hello from anonymous class");
            }
        };
        greeting.greet();
    }
}

// Or extending a class
class Outer {
    void method() {
        Thread thread = new Thread() {
            @Override
            public void run() {
                System.out.println("Thread running");
            }
        };
        thread.start();
    }
}
```

## Accessing Outer Class Members
```java
class Outer {
    private int outerField = 10;
    private static int staticField = 20;
    
    class Inner {
        void display() {
            // Access instance field
            System.out.println(outerField);
            
            // Access static field
            System.out.println(staticField);
            
            // Access outer class method
            outerMethod();
        }
    }
    
    void outerMethod() {
        System.out.println("Outer method");
    }
}
```

## When to Use Inner Classes
1. **Logical Grouping**: Related classes together
2. **Encapsulation**: Hide implementation details
3. **Access Control**: Access private members of outer class
4. **Event Handlers**: Anonymous classes for event handling
5. **Callbacks**: Implement callback mechanisms

## Complete Example
```java
class DataStructure {
    private int[] array = {1, 2, 3, 4, 5};
    
    // Inner class
    class Iterator {
        private int index = 0;
        
        boolean hasNext() {
            return index < array.length;
        }
        
        int next() {
            return array[index++];
        }
    }
    
    Iterator getIterator() {
        return new Iterator();
    }
}

// Usage
DataStructure ds = new DataStructure();
DataStructure.Iterator it = ds.getIterator();
while (it.hasNext()) {
    System.out.println(it.next());
}
```


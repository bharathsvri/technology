# Comparable and Comparator

## Comparable Interface
Allows objects to be compared and sorted naturally.

### Implementing Comparable
```java
class Student implements Comparable<Student> {
    private String name;
    private int age;
    
    public Student(String name, int age) {
        this.name = name;
        this.age = age;
    }
    
    @Override
    public int compareTo(Student other) {
        // Compare by age
        return Integer.compare(this.age, other.age);
    }
    
    // Getters
    public String getName() { return name; }
    public int getAge() { return age; }
}
```

### Using Comparable
```java
List<Student> students = new ArrayList<>();
students.add(new Student("Alice", 25));
students.add(new Student("Bob", 20));
students.add(new Student("Charlie", 30));

Collections.sort(students);
// Students sorted by age: Bob(20), Alice(25), Charlie(30)
```

### CompareTo Return Values
- Negative: this object is less than other
- Zero: this object equals other
- Positive: this object is greater than other

## Comparator Interface
Provides external comparison logic, separate from the class.

### Basic Comparator
```java
import java.util.Comparator;

// Compare by name
Comparator<Student> nameComparator = new Comparator<Student>() {
    @Override
    public int compare(Student s1, Student s2) {
        return s1.getName().compareTo(s2.getName());
    }
};

// Usage
Collections.sort(students, nameComparator);
```

### Lambda Comparator (Java 8+)
```java
// Compare by name
Comparator<Student> nameComparator = 
    (s1, s2) -> s1.getName().compareTo(s2.getName());

// Compare by age
Comparator<Student> ageComparator = 
    (s1, s2) -> Integer.compare(s1.getAge(), s2.getAge());

// Using method reference
Comparator<Student> nameComparator2 = 
    Comparator.comparing(Student::getName);
```

### Comparator.comparing()
```java
// Compare by single field
Comparator<Student> byName = Comparator.comparing(Student::getName);
Comparator<Student> byAge = Comparator.comparing(Student::getAge);

// Compare by multiple fields
Comparator<Student> byNameThenAge = 
    Comparator.comparing(Student::getName)
               .thenComparing(Student::getAge);

// Reverse order
Comparator<Student> byAgeDesc = 
    Comparator.comparing(Student::getAge).reversed();
```

### Null Handling
```java
// Nulls first
Comparator<Student> nullsFirst = 
    Comparator.nullsFirst(Comparator.comparing(Student::getName));

// Nulls last
Comparator<Student> nullsLast = 
    Comparator.nullsLast(Comparator.comparing(Student::getName));
```

## Common Patterns

### Natural Order vs Custom Order
```java
// Natural order (using Comparable)
Collections.sort(students);

// Custom order (using Comparator)
Collections.sort(students, Comparator.comparing(Student::getName));
```

### Multiple Comparisons
```java
// Sort by name, then by age
Comparator<Student> multiField = 
    Comparator.comparing(Student::getName)
              .thenComparing(Student::getAge)
              .thenComparing(Student::getId);
```

### Reverse Order
```java
// Reverse natural order
Collections.sort(students, Collections.reverseOrder());

// Reverse custom comparator
Collections.sort(students, 
    Comparator.comparing(Student::getAge).reversed());
```

## Complete Example
```java
import java.util.*;

class Person implements Comparable<Person> {
    private String name;
    private int age;
    private String city;
    
    public Person(String name, int age, String city) {
        this.name = name;
        this.age = age;
        this.city = city;
    }
    
    // Natural ordering by name
    @Override
    public int compareTo(Person other) {
        return this.name.compareTo(other.name);
    }
    
    // Getters
    public String getName() { return name; }
    public int getAge() { return age; }
    public String getCity() { return city; }
    
    @Override
    public String toString() {
        return name + " (" + age + ") from " + city;
    }
}

public class ComparableComparatorDemo {
    public static void main(String[] args) {
        List<Person> people = Arrays.asList(
            new Person("Alice", 25, "NYC"),
            new Person("Bob", 30, "LA"),
            new Person("Charlie", 20, "NYC")
        );
        
        // Sort using natural order (by name)
        Collections.sort(people);
        System.out.println("By name: " + people);
        
        // Sort using Comparator (by age)
        Collections.sort(people, Comparator.comparing(Person::getAge));
        System.out.println("By age: " + people);
        
        // Sort by city, then by age
        Collections.sort(people, 
            Comparator.comparing(Person::getCity)
                      .thenComparing(Person::getAge));
        System.out.println("By city then age: " + people);
        
        // Reverse order
        Collections.sort(people, 
            Comparator.comparing(Person::getAge).reversed());
        System.out.println("By age (desc): " + people);
    }
}
```

## Best Practices
1. Implement Comparable for natural ordering
2. Use Comparator for custom or multiple orderings
3. Use Comparator.comparing() for cleaner code
4. Handle nulls appropriately
5. Make comparisons consistent with equals()
6. Use thenComparing() for multiple fields
7. Consider performance for large collections
8. Document comparison logic

## When to Use Which

### Use Comparable When:
- There's a single, natural ordering
- The class has control over its ordering
- The ordering is fundamental to the class

### Use Comparator When:
- Multiple orderings are needed
- You can't modify the class
- Ordering is not natural to the class
- You need external comparison logic


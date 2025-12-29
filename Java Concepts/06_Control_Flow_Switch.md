# Control Flow - Switch Statement

## Switch Statement
The switch statement allows a variable to be tested for equality against a list of values.

## Basic Syntax
```java
switch (expression) {
    case value1:
        // code block
        break;
    case value2:
        // code block
        break;
    default:
        // code block
}
```

## Example
```java
int day = 3;
switch (day) {
    case 1:
        System.out.println("Monday");
        break;
    case 2:
        System.out.println("Tuesday");
        break;
    case 3:
        System.out.println("Wednesday");
        break;
    case 4:
        System.out.println("Thursday");
        break;
    case 5:
        System.out.println("Friday");
        break;
    default:
        System.out.println("Weekend");
}
```

## Important Points
1. **Expression** must be: byte, short, int, char, String, or enum
2. **Case values** must be constants (literals or final variables)
3. **Break statement** prevents fall-through to next case
4. **Default case** is optional and executes if no match

## Without Break (Fall-Through)
```java
int day = 2;
switch (day) {
    case 1:
    case 2:
    case 3:
    case 4:
    case 5:
        System.out.println("Weekday");
        break;
    case 6:
    case 7:
        System.out.println("Weekend");
        break;
}
```

## Switch with String (Java 7+)
```java
String day = "Monday";
switch (day) {
    case "Monday":
        System.out.println("Start of week");
        break;
    case "Friday":
        System.out.println("End of week");
        break;
    default:
        System.out.println("Midweek");
}
```

## Switch with Enum
```java
enum Day {
    MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY, SATURDAY, SUNDAY
}

Day today = Day.MONDAY;
switch (today) {
    case MONDAY:
        System.out.println("Start of week");
        break;
    case FRIDAY:
        System.out.println("End of week");
        break;
    default:
        System.out.println("Other day");
}
```

## Switch Expression (Java 14+)
Modern switch expression with arrow syntax and yield.

```java
// Traditional switch
int day = 3;
String dayName;
switch (day) {
    case 1: dayName = "Monday"; break;
    case 2: dayName = "Tuesday"; break;
    default: dayName = "Unknown";
}

// Switch expression (Java 14+)
String dayName = switch (day) {
    case 1 -> "Monday";
    case 2 -> "Tuesday";
    case 3 -> "Wednesday";
    default -> "Unknown";
};

// With yield for complex expressions
int result = switch (day) {
    case 1, 2, 3, 4, 5 -> {
        System.out.println("Weekday");
        yield day * 2;
    }
    case 6, 7 -> {
        System.out.println("Weekend");
        yield day * 3;
    }
    default -> 0;
};
```

## Multiple Case Labels (Java 14+)
```java
int day = 2;
switch (day) {
    case 1, 2, 3, 4, 5 -> System.out.println("Weekday");
    case 6, 7 -> System.out.println("Weekend");
    default -> System.out.println("Invalid");
}
```

## Pattern Matching (Java 21+)
```java
Object obj = "Hello";
String result = switch (obj) {
    case String s -> "String: " + s;
    case Integer i -> "Integer: " + i;
    case null -> "Null value";
    default -> "Unknown type";
};
```

## When to Use Switch vs If-Else
- **Use Switch**: When checking a single variable against multiple constant values
- **Use If-Else**: When checking complex conditions, ranges, or multiple variables

```java
// Good for switch
int month = 3;
switch (month) {
    case 1: case 3: case 5: case 7: case 8: case 10: case 12:
        System.out.println("31 days");
        break;
    case 4: case 6: case 9: case 11:
        System.out.println("30 days");
        break;
    case 2:
        System.out.println("28 or 29 days");
        break;
}

// Better with if-else
int score = 85;
if (score >= 90 && score <= 100) {
    System.out.println("A");
} else if (score >= 80 && score < 90) {
    System.out.println("B");
}
```


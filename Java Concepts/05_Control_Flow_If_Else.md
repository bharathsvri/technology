# Control Flow - If-Else Statements

## If Statement
Executes code block if condition is true.

```java
if (condition) {
    // code to execute if condition is true
}
```

### Example
```java
int age = 18;
if (age >= 18) {
    System.out.println("You are an adult");
}
```

## If-Else Statement
Executes one block if condition is true, another if false.

```java
if (condition) {
    // code if condition is true
} else {
    // code if condition is false
}
```

### Example
```java
int age = 16;
if (age >= 18) {
    System.out.println("You are an adult");
} else {
    System.out.println("You are a minor");
}
```

## If-Else-If Ladder
Checks multiple conditions sequentially.

```java
if (condition1) {
    // code if condition1 is true
} else if (condition2) {
    // code if condition2 is true
} else if (condition3) {
    // code if condition3 is true
} else {
    // code if all conditions are false
}
```

### Example
```java
int score = 85;
if (score >= 90) {
    System.out.println("Grade: A");
} else if (score >= 80) {
    System.out.println("Grade: B");
} else if (score >= 70) {
    System.out.println("Grade: C");
} else if (score >= 60) {
    System.out.println("Grade: D");
} else {
    System.out.println("Grade: F");
}
```

## Nested If-Else
If-else statements inside other if-else statements.

```java
int age = 20;
boolean hasLicense = true;

if (age >= 18) {
    if (hasLicense) {
        System.out.println("You can drive");
    } else {
        System.out.println("You need a license");
    }
} else {
    System.out.println("You are too young to drive");
}
```

## Ternary Operator (Alternative to If-Else)
Shorthand for simple if-else statements.

```java
// Instead of:
int max;
if (a > b) {
    max = a;
} else {
    max = b;
}

// Use:
int max = (a > b) ? a : b;
```

## Common Patterns

### Checking Multiple Conditions
```java
int age = 25;
boolean isStudent = false;

if (age >= 18 && !isStudent) {
    System.out.println("Full price ticket");
} else if (age < 18 || isStudent) {
    System.out.println("Discounted ticket");
}
```

### Null Checking
```java
String name = null;
if (name != null && name.length() > 0) {
    System.out.println("Name: " + name);
} else {
    System.out.println("Name is empty or null");
}
```

### Boolean Variables
```java
boolean isActive = true;
if (isActive) {
    System.out.println("Account is active");
}

// Equivalent to:
if (isActive == true) {
    System.out.println("Account is active");
}
```

## Best Practices
1. Use meaningful condition names
2. Avoid deeply nested if-else (consider switch or refactoring)
3. Always use braces `{}` even for single statements
4. Order conditions from most specific to least specific
5. Use early returns to reduce nesting

```java
// Good: Early return
public String getStatus(int value) {
    if (value < 0) return "Negative";
    if (value == 0) return "Zero";
    return "Positive";
}

// Avoid: Deep nesting
public String getStatus(int value) {
    if (value >= 0) {
        if (value == 0) {
            return "Zero";
        } else {
            return "Positive";
        }
    } else {
        return "Negative";
    }
}
```


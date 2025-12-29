# Java Methods

## What is a Method?
A method is a block of code that performs a specific task and can be reused. It helps in code reusability and organization.

## Method Declaration
```java
accessModifier returnType methodName(parameters) {
    // method body
    return value; // if returnType is not void
}
```

## Method Components
1. **Access Modifier**: public, private, protected, or default
2. **Return Type**: Data type of value returned (void if nothing returned)
3. **Method Name**: Identifier for the method
4. **Parameters**: Input values (optional)
5. **Method Body**: Code that executes

## Simple Method Example
```java
public class Calculator {
    // Method with no parameters and no return
    public void displayMessage() {
        System.out.println("Hello from method!");
    }
    
    // Method with parameters and return value
    public int add(int a, int b) {
        return a + b;
    }
    
    // Method call
    public static void main(String[] args) {
        Calculator calc = new Calculator();
        calc.displayMessage();
        int result = calc.add(5, 3);
        System.out.println("Sum: " + result);
    }
}
```

## Method Types

### 1. No Parameters, No Return
```java
public void greet() {
    System.out.println("Hello!");
}
```

### 2. With Parameters, No Return
```java
public void greet(String name) {
    System.out.println("Hello, " + name + "!");
}
```

### 3. No Parameters, With Return
```java
public String getMessage() {
    return "Welcome!";
}
```

### 4. With Parameters, With Return
```java
public int multiply(int a, int b) {
    return a * b;
}
```

## Method Overloading
Multiple methods with same name but different parameters.

```java
public class MathUtils {
    // Overloaded methods
    public int add(int a, int b) {
        return a + b;
    }
    
    public int add(int a, int b, int c) {
        return a + b + c;
    }
    
    public double add(double a, double b) {
        return a + b;
    }
    
    // Method calls
    add(5, 3);        // Calls first method
    add(5, 3, 2);     // Calls second method
    add(5.5, 3.2);    // Calls third method
}
```

## Static Methods
Belong to the class, not instances. Called using class name.

```java
public class MathUtils {
    public static int square(int num) {
        return num * num;
    }
    
    // Call without creating object
    int result = MathUtils.square(5);
}
```

## Instance Methods
Belong to instances of the class. Called using object reference.

```java
public class Person {
    private String name;
    
    public void setName(String name) {
        this.name = name;
    }
    
    public String getName() {
        return name;
    }
    
    // Usage
    Person person = new Person();
    person.setName("John");
    String name = person.getName();
}
```

## Method Parameters

### Pass by Value (Primitives)
```java
public void modifyValue(int x) {
    x = 100; // Changes local copy only
}

int num = 10;
modifyValue(num);
System.out.println(num); // Still 10
```

### Pass by Reference (Objects)
```java
public void modifyArray(int[] arr) {
    arr[0] = 100; // Modifies original array
}

int[] numbers = {1, 2, 3};
modifyArray(numbers);
System.out.println(numbers[0]); // 100
```

## Variable Arguments (Varargs)
Method that accepts variable number of arguments.

```java
public int sum(int... numbers) {
    int total = 0;
    for (int num : numbers) {
        total += num;
    }
    return total;
}

// Usage
sum(1, 2, 3);        // 6
sum(1, 2, 3, 4, 5);  // 15
```

## Return Statement
```java
public int getMax(int a, int b) {
    if (a > b) {
        return a;
    }
    return b; // Must return value if method has return type
}

// void methods can use return to exit early
public void process(int value) {
    if (value < 0) {
        return; // Exit method early
    }
    // Continue processing
}
```

## Recursive Methods
Method that calls itself.

```java
public int factorial(int n) {
    if (n <= 1) {
        return 1; // Base case
    }
    return n * factorial(n - 1); // Recursive call
}

// Usage
int result = factorial(5); // 120
```

## Method Best Practices
1. **Single Responsibility**: Each method should do one thing
2. **Meaningful Names**: Use descriptive method names
3. **Keep Methods Short**: Aim for 20-30 lines maximum
4. **Avoid Too Many Parameters**: Consider using objects for many parameters
5. **Document Complex Logic**: Add comments for complex algorithms
6. **Use Appropriate Access Modifiers**: Make methods private if only used internally

## Example: Complete Class with Methods
```java
public class BankAccount {
    private double balance;
    
    // Constructor
    public BankAccount(double initialBalance) {
        this.balance = initialBalance;
    }
    
    // Getter method
    public double getBalance() {
        return balance;
    }
    
    // Setter method
    public void deposit(double amount) {
        if (amount > 0) {
            balance += amount;
        }
    }
    
    // Method with validation
    public boolean withdraw(double amount) {
        if (amount > 0 && amount <= balance) {
            balance -= amount;
            return true;
        }
        return false;
    }
    
    // Static utility method
    public static double calculateInterest(double principal, double rate, int years) {
        return principal * rate * years / 100;
    }
}
```


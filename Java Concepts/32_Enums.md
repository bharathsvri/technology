# Enums

## What are Enums?
Enums are special classes that represent a group of constants. They provide type safety and better code readability.

## Basic Enum
```java
enum Day {
    MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY, SATURDAY, SUNDAY
}

// Usage
Day today = Day.MONDAY;
System.out.println(today); // MONDAY
```

## Enum with Methods
```java
enum Day {
    MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY, SATURDAY, SUNDAY;
    
    public boolean isWeekend() {
        return this == SATURDAY || this == SUNDAY;
    }
    
    public String getType() {
        return isWeekend() ? "Weekend" : "Weekday";
    }
}

// Usage
Day day = Day.SATURDAY;
System.out.println(day.isWeekend()); // true
System.out.println(day.getType()); // "Weekend"
```

## Enum with Constructors and Fields
```java
enum Planet {
    MERCURY(3.303e+23, 2.4397e6),
    VENUS(4.869e+24, 6.0518e6),
    EARTH(5.976e+24, 6.37814e6),
    MARS(6.421e+23, 3.3972e6);
    
    private final double mass;   // in kilograms
    private final double radius;  // in meters
    
    Planet(double mass, double radius) {
        this.mass = mass;
        this.radius = radius;
    }
    
    public double getMass() {
        return mass;
    }
    
    public double getRadius() {
        return radius;
    }
    
    public double surfaceGravity() {
        double G = 6.67300E-11;
        return G * mass / (radius * radius);
    }
}

// Usage
Planet earth = Planet.EARTH;
System.out.println(earth.getMass());
System.out.println(earth.surfaceGravity());
```

## Enum with Switch
```java
enum Status {
    PENDING, APPROVED, REJECTED
}

Status status = Status.APPROVED;

switch (status) {
    case PENDING:
        System.out.println("Waiting for approval");
        break;
    case APPROVED:
        System.out.println("Request approved");
        break;
    case REJECTED:
        System.out.println("Request rejected");
        break;
}
```

## Enum Methods
```java
enum Color {
    RED, GREEN, BLUE
}

Color color = Color.RED;

// Get name
String name = color.name(); // "RED"

// Get ordinal
int ordinal = color.ordinal(); // 0

// Compare
int comparison = color.compareTo(Color.GREEN); // negative

// Get all values
Color[] allColors = Color.values();

// Value of
Color red = Color.valueOf("RED");
```

## Enum Implementing Interface
```java
interface Operation {
    double apply(double x, double y);
}

enum Calculator implements Operation {
    PLUS {
        public double apply(double x, double y) {
            return x + y;
        }
    },
    MINUS {
        public double apply(double x, double y) {
            return x - y;
        }
    },
    MULTIPLY {
        public double apply(double x, double y) {
            return x * y;
        }
    },
    DIVIDE {
        public double apply(double x, double y) {
            return x / y;
        }
    };
}

// Usage
double result = Calculator.PLUS.apply(5, 3); // 8.0
```

## Complete Example
```java
enum HttpStatus {
    OK(200, "OK"),
    NOT_FOUND(404, "Not Found"),
    INTERNAL_ERROR(500, "Internal Server Error");
    
    private final int code;
    private final String message;
    
    HttpStatus(int code, String message) {
        this.code = code;
        this.message = message;
    }
    
    public int getCode() {
        return code;
    }
    
    public String getMessage() {
        return message;
    }
    
    public boolean isSuccess() {
        return code >= 200 && code < 300;
    }
}

// Usage
HttpStatus status = HttpStatus.OK;
System.out.println(status.getCode()); // 200
System.out.println(status.getMessage()); // "OK"
System.out.println(status.isSuccess()); // true
```

## Best Practices
1. Use enums for fixed sets of constants
2. Use descriptive names
3. Add methods and fields when needed
4. Use enums in switch statements
5. Consider implementing interfaces for behavior
6. Use ordinal() sparingly (fragile if order changes)


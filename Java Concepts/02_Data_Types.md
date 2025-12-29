# Java Data Types

## Primitive Data Types

Java has 8 primitive data types:

### Integer Types
- **byte**: 8-bit signed integer (-128 to 127)
- **short**: 16-bit signed integer (-32,768 to 32,767)
- **int**: 32-bit signed integer (-2³¹ to 2³¹-1) - *default for integers*
- **long**: 64-bit signed integer (-2⁶³ to 2⁶³-1)

```java
byte b = 100;
short s = 1000;
int i = 100000;
long l = 10000000000L; // Note the 'L' suffix
```

### Floating-Point Types
- **float**: 32-bit IEEE 754 floating point
- **double**: 64-bit IEEE 754 floating point - *default for decimals*

```java
float f = 3.14f; // Note the 'f' suffix
double d = 3.14159265359;
```

### Character Type
- **char**: 16-bit Unicode character (0 to 65,535)

```java
char c = 'A';
char unicode = '\u0041'; // Unicode for 'A'
```

### Boolean Type
- **boolean**: true or false

```java
boolean isActive = true;
boolean isComplete = false;
```

## Reference Data Types

### String
```java
String name = "Java";
String message = new String("Hello");
```

### Arrays
```java
int[] numbers = {1, 2, 3, 4, 5};
int[] arr = new int[10]; // Array of size 10
```

### Objects
```java
Object obj = new Object();
MyClass instance = new MyClass();
```

## Type Conversion

### Widening (Automatic)
```java
int i = 100;
long l = i; // Automatic conversion
float f = l; // Automatic conversion
```

### Narrowing (Explicit Casting)
```java
double d = 100.04;
long l = (long) d; // Explicit cast
int i = (int) l; // Explicit cast
```

## Wrapper Classes

Each primitive type has a corresponding wrapper class:

| Primitive | Wrapper Class |
|-----------|---------------|
| byte | Byte |
| short | Short |
| int | Integer |
| long | Long |
| float | Float |
| double | Double |
| char | Character |
| boolean | Boolean |

```java
// Autoboxing (automatic conversion)
Integer num = 10; // Primitive to object

// Unboxing (automatic conversion)
int value = num; // Object to primitive
```

## Default Values

| Data Type | Default Value |
|-----------|---------------|
| byte, short, int, long | 0 |
| float, double | 0.0 |
| char | '\u0000' |
| boolean | false |
| Reference types | null |


# Java Operators

## Arithmetic Operators

| Operator | Description | Example |
|----------|-------------|---------|
| + | Addition | `a + b` |
| - | Subtraction | `a - b` |
| * | Multiplication | `a * b` |
| / | Division | `a / b` |
| % | Modulus (remainder) | `a % b` |

```java
int a = 10, b = 3;
System.out.println(a + b); // 13
System.out.println(a - b); // 7
System.out.println(a * b); // 30
System.out.println(a / b); // 3
System.out.println(a % b); // 1
```

## Assignment Operators

| Operator | Description | Equivalent To |
|----------|-------------|---------------|
| = | Assignment | `a = b` |
| += | Add and assign | `a += b` → `a = a + b` |
| -= | Subtract and assign | `a -= b` → `a = a - b` |
| *= | Multiply and assign | `a *= b` → `a = a * b` |
| /= | Divide and assign | `a /= b` → `a = a / b` |
| %= | Modulus and assign | `a %= b` → `a = a % b` |

```java
int x = 10;
x += 5; // x = 15
x -= 3; // x = 12
x *= 2; // x = 24
x /= 4; // x = 6
x %= 4; // x = 2
```

## Relational Operators

| Operator | Description | Example |
|----------|-------------|---------|
| == | Equal to | `a == b` |
| != | Not equal to | `a != b` |
| > | Greater than | `a > b` |
| < | Less than | `a < b` |
| >= | Greater than or equal | `a >= b` |
| <= | Less than or equal | `a <= b` |

```java
int a = 10, b = 20;
System.out.println(a == b); // false
System.out.println(a != b); // true
System.out.println(a > b);  // false
System.out.println(a < b);  // true
```

## Logical Operators

| Operator | Description | Example |
|----------|-------------|---------|
| && | Logical AND | `a && b` |
| \|\| | Logical OR | `a \|\| b` |
| ! | Logical NOT | `!a` |

```java
boolean x = true, y = false;
System.out.println(x && y); // false
System.out.println(x || y); // true
System.out.println(!x);     // false
```

## Bitwise Operators

| Operator | Description | Example |
|----------|-------------|---------|
| & | Bitwise AND | `a & b` |
| \| | Bitwise OR | `a \| b` |
| ^ | Bitwise XOR | `a ^ b` |
| ~ | Bitwise NOT | `~a` |
| << | Left shift | `a << 2` |
| >> | Right shift | `a >> 2` |
| >>> | Unsigned right shift | `a >>> 2` |

```java
int a = 5;  // 0101 in binary
int b = 3;  // 0011 in binary
System.out.println(a & b);  // 1 (0001)
System.out.println(a | b);  // 7 (0111)
System.out.println(a ^ b);  // 6 (0110)
System.out.println(a << 1); // 10 (1010)
System.out.println(a >> 1); // 2 (0010)
```

## Unary Operators

| Operator | Description | Example |
|----------|-------------|---------|
| + | Unary plus | `+a` |
| - | Unary minus | `-a` |
| ++ | Increment | `a++` or `++a` |
| -- | Decrement | `a--` or `--a` |
| ! | Logical complement | `!a` |

```java
int a = 5;
System.out.println(++a); // 6 (pre-increment)
System.out.println(a++); // 6 (post-increment, then a = 7)
System.out.println(--a); // 6 (pre-decrement)
System.out.println(a--); // 6 (post-decrement, then a = 5)
```

## Ternary Operator (Conditional)

```java
// Syntax: condition ? valueIfTrue : valueIfFalse
int a = 10, b = 20;
int max = (a > b) ? a : b; // max = 20
```

## Instanceof Operator

Checks if an object is an instance of a class or interface.

```java
String str = "Hello";
System.out.println(str instanceof String); // true
System.out.println(str instanceof Object); // true
```

## Operator Precedence

Operators are evaluated in this order (highest to lowest):
1. Postfix: `expr++`, `expr--`
2. Unary: `++expr`, `--expr`, `+expr`, `-expr`, `!`, `~`
3. Multiplicative: `*`, `/`, `%`
4. Additive: `+`, `-`
5. Shift: `<<`, `>>`, `>>>`
6. Relational: `<`, `>`, `<=`, `>=`, `instanceof`
7. Equality: `==`, `!=`
8. Bitwise AND: `&`
9. Bitwise XOR: `^`
10. Bitwise OR: `|`
11. Logical AND: `&&`
12. Logical OR: `||`
13. Ternary: `? :`
14. Assignment: `=`, `+=`, `-=`, etc.

```java
int result = 2 + 3 * 4; // 14 (not 20)
// Use parentheses to change precedence
int result2 = (2 + 3) * 4; // 20
```


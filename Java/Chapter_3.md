# Chapter 3: Operators
## What are Operators?
Operators are special symbols that perform operations on variables and values. They are the basic building blocks that allow us to manipulate data and make decisions in our programs. Just like in mathematics where + means addition, programming languages use operators to perform various operations.
### Java operators can be classified based on:

* Number of operands (unary, binary, ternary)
* Type of operation (arithmetic, logical, relational, etc.)
* Type of data they work with (numeric, boolean, object)

## Arithmetic Operators
These perform basic mathematical operations. An important distinction in Java is between integer and floating-point arithmetic. When both operands are integers, the result is an integer (truncating any decimal part).
```java 
int a = 10, b = 3;

// Basic operations
System.out.println(a + b);  // 13 (addition)
System.out.println(a - b);  // 7  (subtraction)
System.out.println(a * b);  // 30 (multiplication)
System.out.println(a / b);  // 3  (integer division, not 3.333...)
System.out.println(a % b);  // 1  (remainder/modulus)

// Floating-point division
double x = 10.0, y = 3.0;
System.out.println(x / y);  // 3.333... (floating-point division)
```
## Increment and Decrement Operators
These are unary operators that increase or decrease a value by 1. The position (prefix or postfix) matters when used in expressions.
* Prefix (++x, --x): Operation happens first, then the value is returned
* Postfix (x++, x--): Value is returned first, then operation happens

```java 
int x = 5;
System.out.println(x++);  // Prints 5, then x becomes 6
System.out.println(++x);  // x becomes 7, then prints 7

int y = 10;
int result = y++ * 2;  // result = 20, y = 11
int result2 = ++y * 2; // y = 12, result2 = 24
```

## Assignment Operators
Assignment operators store values in variables. The basic assignment is =, but compound assignments combine an operation with assignment.

Compound assignment operators are not just shortcuts - they also include implicit type casting, which can be convenient.
```java 
int x = 10;

// Compound assignments
x += 5;   // Same as: x = x + 5
x -= 3;   // Same as: x = x - 3
x *= 2;   // Same as: x = x * 2
x /= 4;   // Same as: x = x / 4
x %= 3;   // Same as: x = x % 3

// Implicit casting in compound assignment
byte b = 10;
b += 5;     // Works! Implicit cast
// b = b + 5;  // Error! Would need explicit cast: b = (byte)(b + 5)
```
## Comparison (Relational) Operators
These operators compare two values and return a boolean result (true or false). They're essential for making decisions in your programs.

Important: For objects (including Strings), == compares references (memory addresses), not content. Use .equals() for content comparison.
```java 
int a = 10, b = 20;

System.out.println(a == b);  // false (equal to)
System.out.println(a != b);  // true  (not equal to)
System.out.println(a > b);   // false (greater than)
System.out.println(a < b);   // true  (less than)
System.out.println(a >= b);  // false (greater than or equal)
System.out.println(a <= b);  // true  (less than or equal)

// String comparison trap
String s1 = "Hello";
String s2 = "Hello";
String s3 = new String("Hello");

System.out.println(s1 == s2);      // true  (string pool)
System.out.println(s1 == s3);      // false (different objects)
System.out.println(s1.equals(s3)); // true  (same content)
```
## Logical Operators
Logical operators work with boolean values to create complex conditions. Java uses "short-circuit" evaluation for && and ||, meaning it stops evaluating as soon as the result is determined.
```java 
boolean x = true, y = false;

// AND (&&) - both must be true
System.out.println(x && y);  // false

// OR (||) - at least one must be true
System.out.println(x || y);  // true

// NOT (!) - reverses the boolean
System.out.println(!x);      // false

// Short-circuit evaluation
int a = 5;
if (a > 10 && ++a > 5) {
    // Second part never evaluated because first is false
}
System.out.println(a);  // Still 5, not 6
```
## Bitwise Operators
These operate on individual bits of integer values. While less common in everyday programming, they're essential for low-level operations and optimizations.
```java 
int a = 5;   // Binary: 0101
int b = 3;   // Binary: 0011

System.out.println(a & b);   // 1  (AND: 0001)
System.out.println(a | b);   // 7  (OR: 0111)
System.out.println(a ^ b);   // 6  (XOR: 0110)
System.out.println(~a);      // -6 (NOT: inverts all bits)
System.out.println(a << 1);  // 10 (left shift = multiply by 2)
System.out.println(a >> 1);  // 2  (right shift = divide by 2)
```
## Ternary Operator
The only operator that takes three operands. It's a compact way to write simple if-else statements.
Syntax: condition ? valueIfTrue : valueIfFalse

```java 
int age = 18;
String status = (age >= 18) ? "Adult" : "Minor";

// Finding maximum
int a = 10, b = 20;
int max = (a > b) ? a : b;

// Nested ternary (use sparingly - can be hard to read)
String grade = (score >= 90) ? "A" :
               (score >= 80) ? "B" :
               (score >= 70) ? "C" : "F";
```

## Operator Precedence
When multiple operators appear in an expression, precedence determines the order of evaluation. Use parentheses to make your intent clear and avoid confusion.
```java 
// Without parentheses - follows precedence
int result = 10 + 20 * 30;     // 610 (not 900)

// With parentheses - overrides precedence
int result2 = (10 + 20) * 30;  // 900

// Precedence order (simplified):
// 1. Parentheses ()
// 2. Unary operators (++, --, !, ~, +, -)
// 3. Multiplication, Division, Modulus (*, /, %)
// 4. Addition, Subtraction (+, -)
// 5. Relational (<, >, <=, >=)
// 6. Equality (==, !=)
// 7. Logical AND (&&)
// 8. Logical OR (||)
// 9. Ternary (? :)
// 10. Assignment (=, +=, -=, etc.)
```

# Chapter 2: Variables and Data Types
## Understanding Variables
A variable is a container that holds data that can be changed during program execution. Think of it as a labeled box where you store values. 

In Java, every variable must be declared with a specific data type before it can be used. This is called static typing, and it helps catch errors at compile-time rather than runtime.

Unlike dynamically-typed languages like Python where you can write x = 5 and later x = "hello", Java requires you to specify what type of data a variable will hold, and it can only hold that type of data throughout its lifetime.

Variable Declaration and Initialization
Declaration is telling Java to reserve memory for a variable of a specific type. Initialization is giving that variable its first value. You can do these separately or together.
```java
// Declaration only
int age;

// Initialization (must declare first)
age = 25;

// Declaration and initialization together
int score = 100;

// Multiple declarations
int x, y, z;
int a = 5, b = 10;
```
## Naming Conventions
Java has strict rules for variable names:

* Must start with a letter, underscore (_), or dollar sign ($)
* Can contain letters, digits, underscores, and dollar signs
* Cannot use Java keywords (like class, public, if, etc.)
* Case-sensitive (age and Age are different variables)

## Convention:

* Variables use camelCase: firstName, accountBalance
* Constants use UPPER_SNAKE_CASE: MAX_SIZE, PI
* Classes use PascalCase: StudentRecord, BankAccount

## Primitive Data Types
Java has 8 primitive data types that store simple values directly in memory. They're called "primitive" because they're not objects - they hold the actual value, not a reference to it.

1. Integer Types

    Integers are whole numbers without decimal points. Java provides four integer types based on the range of values you need:
    1. byte (8 bits): Range from -128 to 127. Used when memory is critical and values are small.

    2. short (16 bits): Range from -32,768 to 32,767. Rarely used in modern applications.

    3. int (32 bits): Range from -2,147,483,648 to 2,147,483,647. This is the default and most commonly used integer type.

    4. long (64 bits): Range approximately ±9.2 quintillion. Used for very large numbers like timestamps or file sizes. Requires 'L' suffix for literals.
    
    ```java
    byte temperature = -10;
    short year = 2024;
    int population = 1000000;
    long distance = 384400000L;  // Note the 'L' suffix
    ```

2. Floating-Point Types
    These store numbers with decimal points. Java uses IEEE 754 standard for floating-point representation.
    1. float (32 bits): Approximately 7 decimal digits of precision. Requires 'f' suffix for literals. Less commonly used due to limited precision.

    2. double (64 bits): Approximately 15 decimal digits of precision. Default type for decimals and recommended for most use cases.

    ```java
    float price = 19.99f;  // Note the 'f' suffix
    double pi = 3.141592653589793;
    double scientific = 1.23e-4;  // 0.000123
    ```
3. Character Type

    char (16 bits): Stores a single Unicode character. Uses single quotes. Java uses UTF-16 encoding, allowing it to represent characters from virtually all world languages.
    ```java 
    char grade = 'A';
    char symbol = '@';
    char unicode = '\u0041';  // Unicode for 'A'
    char newline = '\n';      // Escape sequence
    ```
4. Boolean Type

    boolean: Can only be true or false. Used for logical operations and control flow. Unlike some languages, Java doesn't treat 0 as false or 1 as true.
    ```java
    boolean isActive = true;
    boolean isComplete = false;
    boolean result = 10 > 5;  // true
    ```
### Reference Types- (call by value and call by reference)

Reference types don't store the actual data but rather a reference (memory address) to where the data is stored. All non-primitive types in Java are reference types.

The most important distinction: when you assign a primitive variable to another, the value is copied. When you assign a reference variable to another, the reference is copied, not the object itself.
 ```java 
// String - most common reference type
String name = "John";
String empty = "";
String nullStr = null;  // No object, just null reference

// Arrays are reference types
int[] numbers = {1, 2, 3, 4, 5};
String[] words = new String[10];

// Objects
Scanner scanner = new Scanner(System.in);
Date today = new Date();

// Demonstrating reference behavior
int[] arr1 = {1, 2, 3};
 int[] arr2 = arr1;  // Both reference same array
arr2[0] = 99;        // Changes arr1[0] too!
```

## Type Conversion
Java is strongly typed, but it allows conversion between compatible types.

### Implicit Conversion (Widening)
Automatic conversion happens when there's no risk of data loss, going from smaller to larger types:
byte → short → int → long → float → double
```java 
byte b = 100;
int i = b;        // Automatic: byte to int
long l = i;       // Automatic: int to long
double d = l;     // Automatic: long to double
```
### Explicit Conversion (Casting)
When converting from larger to smaller types (narrowing), you must explicitly cast, accepting potential data loss:
```java
double pi = 3.14159;
int approx = (int) pi;  // 3 (decimal part lost)

int large = 300;
byte small = (byte) large;  // Overflow! Result: 44

// Casting in expressions
int x = 7, y = 2;
double result = (double) x / y;  // 3.5 (not 3)
```
## Variable Scope and Lifetime
Scope determines where a variable can be accessed, and lifetime determines how long it exists in memory.

* Local Variables: Declared inside methods, constructors, or blocks. They exist only while that block is executing and must be initialized before use.
* Instance Variables: Declared inside a class but outside methods. Each object gets its own copy. They're initialized with default values if not explicitly initialized.
* Static Variables: Declared with the static keyword. Belong to the class itself, not instances. Shared among all objects of the class.
```java 
public class ScopeDemo {
    // Static variable - class level, shared
    static int count = 0;
    
    // Instance variable - each object has its own
    int id;
    
    public void method() {
        // Local variable - only exists in this method
        int local = 10;
        
        if (local > 5) {
            // Block scope - only exists in this if block
            int temp = 20;
        }
        // temp is not accessible here
    }
}
```
## Constants
Constants are variables whose values cannot change after initialization. Use the final keyword to declare constants. By convention, constant names are in uppercase.

```java 
final double PI = 3.14159;
final int MAX_USERS = 100;
final String APP_NAME = "MyApp";

// PI = 3.14;  // Error! Cannot reassign
```

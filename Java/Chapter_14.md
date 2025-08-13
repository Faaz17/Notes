# Chapter 14: Strings
## Understanding Strings in Java
Strings in Java are objects that represent sequences of characters. Unlike primitive types, String is a reference type, but it has some special characteristics that make it behave differently from other objects.

Key characteristics of Strings:

* Immutable: Once created, a String cannot be changed
* String Pool: Java maintains a pool of string literals for memory efficiency
* Special Syntax: Can be created using literals like primitives
* Thread-Safe: Because they're immutable, strings are inherently thread-safe

## String Creation and String Pool
Java maintains a special memory area called the String Pool (or String Constant Pool) in the heap memory. When you create a string literal, Java first checks if that string already exists in the pool.
```java 
// String literal - stored in String Pool
String str1 = "Hello";
String str2 = "Hello";

// Both refer to same object in pool
System.out.println(str1 == str2);  // true

// Using new keyword - creates new object in heap
String str3 = new String("Hello");
System.out.println(str1 == str3);  // false (different objects)
System.out.println(str1.equals(str3));  // true (same content)

// Intern method - adds to pool or returns from pool
String str4 = new String("World").intern();
String str5 = "World";
System.out.println(str4 == str5);  // true
```
## String Immutability
Strings cannot be changed after creation. Any operation that seems to modify a string actually creates a new string object.
```java 
String original = "Hello";
String modified = original.concat(" World");

System.out.println(original);  // Still "Hello"
System.out.println(modified);  // "Hello World"

// Why immutability matters
String str = "Java";
str.toUpperCase();  // Creates new string, doesn't modify str
System.out.println(str);  // Still "Java"

str = str.toUpperCase();  // Must reassign
System.out.println(str);  // Now "JAVA"
```
## Common String Methods\
```java 
String str = "Hello World";

// Length and character access
int length = str.length();              // 11
char ch = str.charAt(0);               // 'H'
int index = str.indexOf("World");      // 6
int lastIndex = str.lastIndexOf('o');  // 7

// Substring
String sub1 = str.substring(6);        // "World"
String sub2 = str.substring(0, 5);     // "Hello"

// Case conversion
String upper = str.toUpperCase();      // "HELLO WORLD"
String lower = str.toLowerCase();      // "hello world"

// Trimming and replacing
String padded = "  Java  ";
String trimmed = padded.trim();        // "Java"
String replaced = str.replace('o', 'a');  // "Hella Warld"
String replacedStr = str.replace("World", "Java");  // "Hello Java"

// Checking content
boolean starts = str.startsWith("Hello");  // true
boolean ends = str.endsWith("World");      // true
boolean contains = str.contains("lo");     // true
boolean empty = str.isEmpty();             // false

// Splitting
String csv = "apple,banana,orange";
String[] fruits = csv.split(",");      // ["apple", "banana", "orange"]

// Joining (Java 8+)
String joined = String.join("-", "2024", "03", "15");  // "2024-03-15"
```
## String Comparison
String comparison requires careful attention because of the difference between reference equality and content equality.
```java 
String s1 = "Hello";
String s2 = "Hello";
String s3 = new String("Hello");
String s4 = "hello";

// Reference comparison (==)
System.out.println(s1 == s2);   // true (same object in pool)
System.out.println(s1 == s3);   // false (different objects)

// Content comparison (equals)
System.out.println(s1.equals(s2));  // true
System.out.println(s1.equals(s3));  // true
System.out.println(s1.equals(s4));  // false (case-sensitive)

// Case-insensitive comparison
System.out.println(s1.equalsIgnoreCase(s4));  // true

// Lexicographic comparison
int result = s1.compareTo(s4);  // Negative (H < h in ASCII)
// Returns: negative if s1 < s4, zero if equal, positive if s1 > s4
```
## StringBuilder and StringBuffer
Since String is immutable, frequent string modifications can be inefficient. StringBuilder (not thread-safe) and StringBuffer (thread-safe) provide mutable alternatives.
```java 
// Inefficient with String
String result = "";
for (int i = 0; i < 1000; i++) {
    result += "a";  // Creates new String each time
}

// Efficient with StringBuilder
StringBuilder sb = new StringBuilder();
for (int i = 0; i < 1000; i++) {
    sb.append("a");  // Modifies existing buffer
}
String result = sb.toString();

// StringBuilder methods
StringBuilder builder = new StringBuilder("Hello");
builder.append(" World");           // "Hello World"
builder.insert(5, ",");            // "Hello, World"
builder.delete(5, 6);              // "Hello World"
builder.reverse();                 // "dlroW olleH"
builder.setCharAt(0, 'D');         // "DlroW olleH"

// StringBuffer (thread-safe version)
StringBuffer buffer = new StringBuffer("Java");
buffer.append(" Programming");
// Same methods as StringBuilder but synchronized
```
## String Formatting
Java provides several ways to format strings:
```java 
// String.format() method
String formatted = String.format("Name: %s, Age: %d", "John", 25);
String decimal = String.format("Price: %.2f", 19.99);

// printf for direct printing
System.out.printf("Total: $%.2f%n", 99.99);

// Format specifiers
// %d - integer
// %f - floating point
// %s - string
// %c - character
// %b - boolean
// %n - newline

// Text blocks (Java 13+)
String textBlock = """
    {
        "name": "John",
        "age": 30
    }
    """;
```
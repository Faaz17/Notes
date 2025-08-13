# Chapter 17: Lambda Expressions
## Understanding Lambda Expressions
Lambda expressions, introduced in Java 8, represent one of the biggest changes to the Java language. They provide a clear and concise way to implement single-method interfaces (functional interfaces) using an expression. Before lambdas, you would need to use anonymous inner classes, which were verbose and harder to read.

A lambda expression is essentially an anonymous function - a method without a name that can be passed around as if it was an object. They enable you to treat functionality as a method argument, or code as data. This is the foundation of functional programming in Java.

The basic syntax of a lambda expression is:
(parameters) -> expression
or
(parameters) -> { statements; }

Lambda expressions are particularly useful when:

* You need to pass a simple functionality as an argument to another method
* You want to write more readable and maintainable code
* You're working with collections and streams
* You need to implement event handlers or callbacks

## Lambda Syntax Variations
Lambda expressions can take different forms depending on the context. The compiler is smart enough to infer types in most cases, making the syntax very flexible.
```java 
// No parameters
() -> System.out.println("Hello World")

// One parameter (parentheses optional)
x -> x * 2
(x) -> x * 2

// Multiple parameters (parentheses required)
(x, y) -> x + y

// With explicit type declaration
(int x, int y) -> x + y

// Multiple statements (requires braces and return)
(x, y) -> {
    int sum = x + y;
    System.out.println("Sum calculated");
    return sum;
}

// Returning void
(String s) -> System.out.println(s)
```
## Functional Interfaces
A functional interface is an interface that contains exactly one abstract method. They can have multiple default or static methods, but only one abstract method. Lambda expressions can only be used where a functional interface is expected.

Java 8 introduced the @FunctionalInterface annotation to explicitly mark interfaces as functional. While optional, it's good practice as it helps catch errors at compile time if someone accidentally adds another abstract method.
```java 
// Custom functional interface
@FunctionalInterface
interface Calculator {
    int calculate(int a, int b);
}

// Using lambda with custom interface
public class LambdaExample {
    public static void main(String[] args) {
        Calculator add = (a, b) -> a + b;
        Calculator subtract = (a, b) -> a - b;
        Calculator multiply = (a, b) -> a * b;
        Calculator divide = (a, b) -> {
            if (b == 0) throw new ArithmeticException("Division by zero");
            return a / b;
        };
        
        System.out.println(add.calculate(10, 5));      // 15
        System.out.println(subtract.calculate(10, 5)); // 5
        System.out.println(multiply.calculate(10, 5)); // 50
        System.out.println(divide.calculate(10, 5));   // 2
    }
}
```
## Built-in Functional Interfaces
Java 8 introduced several built-in functional interfaces in the java.util.function package. These cover most common use cases and promote consistency across Java APIs.
```java 
import java.util.function.*;

// Predicate<T> - takes one argument, returns boolean
Predicate<Integer> isEven = n -> n % 2 == 0;
Predicate<String> isEmpty = String::isEmpty;
System.out.println(isEven.test(4));  // true
System.out.println(isEmpty.test("")); // true

// Function<T, R> - takes one argument of type T, returns type R
Function<String, Integer> stringLength = s -> s.length();
Function<Integer, String> intToString = i -> "Number: " + i;
System.out.println(stringLength.apply("Hello"));  // 5
System.out.println(intToString.apply(42));        // "Number: 42"

// Consumer<T> - takes one argument, returns nothing (side effects)
Consumer<String> printer = s -> System.out.println("Printing: " + s);
Consumer<List<Integer>> listModifier = list -> list.add(100);
printer.accept("Hello");

// Supplier<T> - takes no arguments, returns a value
Supplier<Double> randomSupplier = Math::random;
Supplier<String> stringSupplier = () -> "Hello World";
System.out.println(randomSupplier.get());

// BiFunction<T, U, R> - takes two arguments, returns a value
BiFunction<Integer, Integer, Integer> max = (a, b) -> a > b ? a : b;
BiFunction<String, String, String> concat = (s1, s2) -> s1 + s2;
System.out.println(max.apply(10, 20));           // 20
System.out.println(concat.apply("Hello", "World")); // "HelloWorld"

// UnaryOperator<T> - special Function where input and output are same type
UnaryOperator<Integer> square = x -> x * x;
UnaryOperator<String> toUpper = String::toUpperCase;
System.out.println(square.apply(5));        // 25
System.out.println(toUpper.apply("hello")); // "HELLO"

// BinaryOperator<T> - special BiFunction where both inputs and output are same type
BinaryOperator<Integer> sum = (a, b) -> a + b;
BinaryOperator<String> stringConcat = String::concat;
System.out.println(sum.apply(10, 20));     // 30
```
## Lambda Expressions with Collections
Lambda expressions revolutionize how we work with collections, making operations much more concise and readable.
```java 
import java.util.*;

List<String> names = Arrays.asList("Alice", "Bob", "Charlie", "David", "Eve");

// forEach - iterate over collection
names.forEach(name -> System.out.println("Hello, " + name));
names.forEach(System.out::println);  // Method reference

// removeIf - remove elements matching condition
List<Integer> numbers = new ArrayList<>(Arrays.asList(1, 2, 3, 4, 5, 6));
numbers.removeIf(n -> n % 2 == 0);  // Remove all even numbers
System.out.println(numbers);  // [1, 3, 5]

// sort - custom sorting
List<String> fruits = new ArrayList<>(Arrays.asList("banana", "apple", "cherry"));
fruits.sort((s1, s2) -> s1.compareTo(s2));  // Alphabetical order
fruits.sort((s1, s2) -> s2.length() - s1.length());  // By length descending

// Custom object sorting
class Person {
    String name;
    int age;
    
    Person(String name, int age) {
        this.name = name;
        this.age = age;
    }
}

List<Person> people = new ArrayList<>();
people.add(new Person("Alice", 30));
people.add(new Person("Bob", 25));
people.add(new Person("Charlie", 35));

// Sort by age
people.sort((p1, p2) -> p1.age - p2.age);

// Using Comparator methods
people.sort(Comparator.comparing(p -> p.name));
people.sort(Comparator.comparingInt(p -> p.age).reversed());

// replaceAll - transform all elements
List<String> words = new ArrayList<>(Arrays.asList("hello", "world"));
words.replaceAll(s -> s.toUpperCase());
System.out.println(words);  // [HELLO, WORLD]
```
## Method References
Method references are a shorthand notation for lambda expressions that only call an existing method. They make code even more readable when the lambda expression is simply delegating to an existing method.

There are four types of method references:
```java 
// 1. Reference to a static method
// ClassName::staticMethodName
Function<String, Integer> parser = Integer::parseInt;
// Equivalent to: s -> Integer.parseInt(s)

List<String> stringNumbers = Arrays.asList("1", "2", "3");
List<Integer> integers = stringNumbers.stream()
    .map(Integer::parseInt)
    .collect(Collectors.toList());

// 2. Reference to an instance method of a particular object
// object::instanceMethodName
String prefix = "Hello ";
Function<String, String> prefixer = prefix::concat;
// Equivalent to: s -> prefix.concat(s)

// 3. Reference to an instance method of an arbitrary object of a particular type
// ClassName::instanceMethodName
Function<String, String> toUpperCase = String::toUpperCase;
// Equivalent to: s -> s.toUpperCase()

List<String> words = Arrays.asList("hello", "world");
words.stream().map(String::toUpperCase).forEach(System.out::println);

// 4. Reference to a constructor
// ClassName::new
Supplier<ArrayList> listFactory = ArrayList::new;
// Equivalent to: () -> new ArrayList()

Function<Integer, ArrayList> sizedListFactory = ArrayList::new;
// Equivalent to: size -> new ArrayList(size)
```
## Variable Scope and Effectively Final
Lambda expressions can access variables from their enclosing scope, but there are important restrictions. Local variables accessed by lambda expressions must be final or effectively final (not modified after initialization).
```java 
public class LambdaScope {
    private int instanceVar = 10;
    private static int staticVar = 20;
    
    public void demonstrateScope() {
        int localVar = 30;  // Effectively final
        int modifiedVar = 40;
        modifiedVar = 50;  // Modified, so not effectively final
        
        // Lambda can access all variables
        Runnable r = () -> {
            // Can read and modify instance variables
            System.out.println(instanceVar);
            instanceVar++;
            
            // Can read and modify static variables
            System.out.println(staticVar);
            staticVar++;
            
            // Can only read effectively final local variables
            System.out.println(localVar);
            // localVar++;  // Compilation error!
            
            // Cannot access non-effectively final variables
            // System.out.println(modifiedVar);  // Compilation error!
        };
        
        r.run();
    }
    
    // Lambda in a loop - common pitfall
    public List<Runnable> createRunnables() {
        List<Runnable> runnables = new ArrayList<>();
        
        for (int i = 0; i < 5; i++) {
            int finalI = i;  // Create effectively final copy
            runnables.add(() -> System.out.println(finalI));
        }
        
        return runnables;
    }
}
```
## Combining Functional Interfaces
Many functional interfaces provide default methods that allow you to combine or modify their behavior:
```java 
import java.util.function.*;

// Combining Predicates
Predicate<Integer> isEven = n -> n % 2 == 0;
Predicate<Integer> isPositive = n -> n > 0;

// AND combination
Predicate<Integer> isEvenAndPositive = isEven.and(isPositive);
System.out.println(isEvenAndPositive.test(4));   // true
System.out.println(isEvenAndPositive.test(-4));  // false

// OR combination
Predicate<Integer> isEvenOrPositive = isEven.or(isPositive);
System.out.println(isEvenOrPositive.test(3));   // true (positive)

// Negation
Predicate<Integer> isOdd = isEven.negate();
System.out.println(isOdd.test(3));  // true

// Combining Functions
Function<Integer, Integer> multiplyBy2 = x -> x * 2;
Function<Integer, Integer> add10 = x -> x + 10;

// andThen - first function then second
Function<Integer, Integer> multiplyThenAdd = multiplyBy2.andThen(add10);
System.out.println(multiplyThenAdd.apply(5));  // (5 * 2) + 10 = 20

// compose - second function then first
Function<Integer, Integer> addThenMultiply = multiplyBy2.compose(add10);
System.out.println(addThenMultiply.apply(5));  // (5 + 10) * 2 = 30

// Combining Consumers
Consumer<String> print = s -> System.out.print(s);
Consumer<String> printNewLine = s -> System.out.println();

Consumer<String> printWithNewLine = print.andThen(printNewLine);
printWithNewLine.accept("Hello");
```
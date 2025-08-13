# Chapter 18: Stream API
## Understanding Streams
The Stream API, introduced in Java 8, represents a major enhancement to the Java Collections Framework. It provides a declarative approach to processing collections of objects. Instead of using loops and conditional statements to process data, you describe what you want to achieve, and the Stream API handles the how.

A stream is not a data structure; it's a sequence of elements from a source that supports aggregate operations. Streams bring functional programming to Java, allowing for more concise, readable, and potentially parallel data processing.

Key concepts of streams:

* Pipeline: A stream pipeline consists of a source, zero or more intermediate operations, and a terminal operation
* Lazy Evaluation: Intermediate operations are not executed until a terminal operation is invoked
* Non-Interference: Stream operations should not modify the underlying data source
* Stateless Operations: Preferably, operations should not depend on any state that might change during execution
* Optional Parallelism: Streams can be processed sequentially or in parallel with minimal code changes

Creating Streams
Streams can be created from various sources:
```java 
import java.util.*;
import java.util.stream.*;
import java.nio.file.*;

// 1. From Collections
List<String> list = Arrays.asList("a", "b", "c");
Stream<String> streamFromList = list.stream();
Stream<String> parallelStreamFromList = list.parallelStream();

// 2. From Arrays
int[] array = {1, 2, 3, 4, 5};
IntStream streamFromArray = Arrays.stream(array);
Stream<String> stringStream = Arrays.stream(new String[]{"x", "y", "z"});

// 3. From Static Factory Methods
Stream<String> streamOf = Stream.of("one", "two", "three");
Stream<Integer> streamOfIntegers = Stream.of(1, 2, 3, 4, 5);

// 4. From Stream.builder()
Stream<String> streamBuilder = Stream.<String>builder()
    .add("a")
    .add("b")
    .add("c")
    .build();

// 5. From Stream.generate() - infinite stream
Stream<Double> randomStream = Stream.generate(Math::random).limit(5);
Stream<String> constantStream = Stream.generate(() -> "Echo").limit(3);

// 6. From Stream.iterate() - infinite stream with iteration
Stream<Integer> iteratedStream = Stream.iterate(0, n -> n + 2).limit(10);
// Java 9+ with predicate
Stream<Integer> iterateWithPredicate = Stream.iterate(0, n -> n < 100, n -> n + 10);

// 7. From Primitive Streams
IntStream intStream = IntStream.range(1, 10);        // 1 to 9
IntStream intStreamClosed = IntStream.rangeClosed(1, 10);  // 1 to 10
DoubleStream doubles = DoubleStream.of(1.0, 2.0, 3.0);

// 8. From Files
try {
    Stream<String> lines = Files.lines(Paths.get("file.txt"));
    Stream<Path> paths = Files.walk(Paths.get("."));
} catch (IOException e) {
    e.printStackTrace();
}

// 9. From String
IntStream chars = "hello".chars();
Stream<String> stringStream = Pattern.compile(",").splitAsStream("a,b,c");
```
## Intermediate Operations
Intermediate operations return a stream and can be chained together. They are lazy and are not executed until a terminal operation is invoked.
```java 
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);

// filter - Keep elements matching a predicate
Stream<Integer> evenNumbers = numbers.stream()
    .filter(n -> n % 2 == 0);

// map - Transform each element
Stream<String> numberStrings = numbers.stream()
    .map(n -> "Number: " + n);

// Example: Transform to different type
List<String> words = Arrays.asList("hello", "world", "java");
Stream<Integer> wordLengths = words.stream()
    .map(String::length);

// flatMap - Flatten nested structures
List<List<Integer>> nestedList = Arrays.asList(
    Arrays.asList(1, 2),
    Arrays.asList(3, 4, 5),
    Arrays.asList(6, 7, 8, 9)
);
Stream<Integer> flatStream = nestedList.stream()
    .flatMap(Collection::stream);

// Example: Split sentences into words
List<String> sentences = Arrays.asList("Hello World", "Java Streams");
Stream<String> words2 = sentences.stream()
    .flatMap(sentence -> Arrays.stream(sentence.split(" ")));

// distinct - Remove duplicates
Stream<Integer> uniqueNumbers = Stream.of(1, 2, 2, 3, 3, 3, 4)
    .distinct();

// sorted - Sort elements
Stream<Integer> sortedNumbers = numbers.stream()
    .sorted();

Stream<String> sortedWords = words.stream()
    .sorted(Comparator.comparing(String::length).thenComparing(String::compareTo));

// peek - Perform action without consuming (useful for debugging)
List<Integer> result = numbers.stream()
    .filter(n -> n > 5)
    .peek(n -> System.out.println("Filtered: " + n))
    .map(n -> n * 2)
    .peek(n -> System.out.println("Mapped: " + n))
    .collect(Collectors.toList());

// limit - Limit stream size
Stream<Integer> firstFive = numbers.stream()
    .limit(5);

// skip - Skip elements
Stream<Integer> skipFirstThree = numbers.stream()
    .skip(3);

// takeWhile (Java 9+) - Take while condition is true
Stream<Integer> takeWhileLessThan5 = numbers.stream()
    .takeWhile(n -> n < 5);

// dropWhile (Java 9+) - Drop while condition is true
Stream<Integer> dropWhileLessThan5 = numbers.stream()
    .dropWhile(n -> n < 5);
```
## Terminal Operations
Terminal operations produce a result or side effect and consume the stream. After a terminal operation is performed, the stream pipeline is considered consumed and can no longer be used.
```java 
List<String> names = Arrays.asList("Alice", "Bob", "Charlie", "David", "Eve");
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);

// forEach - Perform action on each element
names.stream()
    .forEach(System.out::println);

// forEachOrdered - Perform action maintaining encounter order (important for parallel streams)
names.parallelStream()
    .forEachOrdered(System.out::println);

// collect - Accumulate elements into a collection
List<String> uppercaseNames = names.stream()
    .map(String::toUpperCase)
    .collect(Collectors.toList());

Set<String> nameSet = names.stream()
    .collect(Collectors.toSet());

// toArray - Convert to array
String[] nameArray = names.stream()
    .toArray(String[]::new);

// reduce - Reduce stream to single value
Optional<Integer> sum = numbers.stream()
    .reduce((a, b) -> a + b);

Integer sumWithIdentity = numbers.stream()
    .reduce(0, (a, b) -> a + b);

String concatenated = names.stream()
    .reduce("", (a, b) -> a + " " + b);

// count - Count elements
long count = names.stream()
    .filter(name -> name.length() > 3)
    .count();

// min/max - Find minimum/maximum
Optional<Integer> min = numbers.stream()
    .min(Integer::compareTo);

Optional<String> maxLengthName = names.stream()
    .max(Comparator.comparing(String::length));

// findFirst/findAny - Find an element
Optional<String> first = names.stream()
    .filter(name -> name.startsWith("C"))
    .findFirst();

Optional<String> any = names.parallelStream()
    .filter(name -> name.length() > 3)
    .findAny();

// anyMatch/allMatch/noneMatch - Check conditions
boolean anyStartsWithA = names.stream()
    .anyMatch(name -> name.startsWith("A"));

boolean allLongerThan2 = names.stream()
    .allMatch(name -> name.length() > 2);

boolean noneEmpty = names.stream()
    .noneMatch(String::isEmpty);
```
## Collectors
The Collectors utility class provides numerous useful reduction operations:
```java 
import java.util.stream.Collectors;
import java.util.*;

class Person {
    String name;
    int age;
    String department;
    double salary;
    
    // Constructor, getters, setters...
}

List<Person> employees = Arrays.asList(
    new Person("Alice", 25, "IT", 50000),
    new Person("Bob", 30, "HR", 45000),
    new Person("Charlie", 25, "IT", 55000),
    new Person("David", 35, "Finance", 60000),
    new Person("Eve", 30, "IT", 52000)
);

// Basic collectors
List<String> namesList = employees.stream()
    .map(Person::getName)
    .collect(Collectors.toList());

Set<String> departments = employees.stream()
    .map(Person::getDepartment)
    .collect(Collectors.toSet());

// Joining strings
String allNames = employees.stream()
    .map(Person::getName)
    .collect(Collectors.joining(", ", "[", "]"));
// Result: "[Alice, Bob, Charlie, David, Eve]"

// Counting
Long count = employees.stream()
    .collect(Collectors.counting());

// Summing/Averaging
Double averageSalary = employees.stream()
    .collect(Collectors.averagingDouble(Person::getSalary));

Integer totalAge = employees.stream()
    .collect(Collectors.summingInt(Person::getAge));

// Statistics
DoubleSummaryStatistics salaryStats = employees.stream()
    .collect(Collectors.summarizingDouble(Person::getSalary));
System.out.println("Average: " + salaryStats.getAverage());
System.out.println("Max: " + salaryStats.getMax());
System.out.println("Min: " + salaryStats.getMin());

// Grouping
Map<String, List<Person>> byDepartment = employees.stream()
    .collect(Collectors.groupingBy(Person::getDepartment));

Map<Integer, List<Person>> byAge = employees.stream()
    .collect(Collectors.groupingBy(Person::getAge));

// Multi-level grouping
Map<String, Map<Integer, List<Person>>> byDeptAndAge = employees.stream()
    .collect(Collectors.groupingBy(
        Person::getDepartment,
        Collectors.groupingBy(Person::getAge)
    ));

// Grouping with downstream collector
Map<String, Long> countByDepartment = employees.stream()
    .collect(Collectors.groupingBy(
        Person::getDepartment,
        Collectors.counting()
    ));

Map<String, Double> avgSalaryByDept = employees.stream()
    .collect(Collectors.groupingBy(
        Person::getDepartment,
        Collectors.averagingDouble(Person::getSalary)
    ));

// Partitioning (special case of grouping with boolean)
Map<Boolean, List<Person>> partitionedBySalary = employees.stream()
    .collect(Collectors.partitioningBy(p -> p.getSalary() > 50000));

// Creating a Map
Map<String, Person> employeeMap = employees.stream()
    .collect(Collectors.toMap(
        Person::getName,
        person -> person
    ));

// Handling duplicates in toMap
Map<Integer, String> ageToNames = employees.stream()
    .collect(Collectors.toMap(
        Person::getAge,
        Person::getName,
        (existing, replacement) -> existing + ", " + replacement
    ));

// Custom reduction
String names = employees.stream()
    .map(Person::getName)
    .collect(Collectors.reducing(
        "",
        (s1, s2) -> s1.isEmpty() ? s2 : s1 + ", " + s2
    ));
```
## Parallel Streams
Parallel streams allow you to leverage multi-core processors for better performance with large datasets:
```java 
// Creating parallel streams
List<Integer> numbers = IntStream.rangeClosed(1, 1000000).boxed().collect(Collectors.toList());

// Method 1: parallelStream()
long sum1 = numbers.parallelStream()
    .mapToLong(Integer::longValue)
    .sum();

// Method 2: parallel() on existing stream
long sum2 = numbers.stream()
    .parallel()
    .mapToLong(Integer::longValue)
    .sum();

// Performance comparison
long startTime = System.currentTimeMillis();
numbers.stream()
    .map(n -> n * 2)
    .filter(n -> n % 3 == 0)
    .collect(Collectors.toList());
long sequentialTime = System.currentTimeMillis() - startTime;

startTime = System.currentTimeMillis();
numbers.parallelStream()
    .map(n -> n * 2)
    .filter(n -> n % 3 == 0)
    .collect(Collectors.toList());
long parallelTime = System.currentTimeMillis() - startTime;

System.out.println("Sequential: " + sequentialTime + "ms");
System.out.println("Parallel: " + parallelTime + "ms");

// When NOT to use parallel streams
// 1. Small datasets - overhead outweighs benefits
// 2. Already in parallel context
// 3. Operations that require ordering
// 4. Operations with shared mutable state

// Thread safety issues with parallel streams
List<Integer> results = new ArrayList<>();
IntStream.range(0, 1000)
    .parallel()
    .forEach(i -> results.add(i));  // NOT thread-safe!

// Correct approach
List<Integer> safeResults = IntStream.range(0, 1000)
    .parallel()
    .boxed()
    .collect(Collectors.toList());  // Thread-safe
```
## Stream Pipeline Best Practices
Here's a comprehensive example showing best practices:
```java
public class StreamPipeline {
    public static void main(String[] args) {
        List<Transaction> transactions = getTransactions();
        
        // Complex pipeline example
        Map<String, Double> highValueTransactionsByCategory = transactions.stream()
            .filter(t -> t.getAmount() > 1000)                    // Filter high value
            .filter(t -> t.getDate().isAfter(LocalDate.now().minusMonths(1))) // Recent only
            .sorted(Comparator.comparing(Transaction::getAmount).reversed())   // Sort by amount
            .limit(100)                                            // Top 100
            .collect(Collectors.groupingBy(
                Transaction::getCategory,
                Collectors.summingDouble(Transaction::getAmount)
            ));
        
        // Debugging with peek
        List<String> processedNames = transactions.stream()
            .peek(t -> System.out.println("Processing: " + t))
            .filter(t -> t.getAmount() > 500)
            .peek(t -> System.out.println("After filter: " + t))
            .map(Transaction::getCustomerName)
            .peek(name -> System.out.println("Customer: " + name))
            .distinct()
            .sorted()
            .collect(Collectors.toList());
        
        // Handling nulls with Optional
        Optional<Transaction> maxTransaction = transactions.stream()
            .filter(Objects::nonNull)
            .max(Comparator.comparing(Transaction::getAmount));
        
        maxTransaction.ifPresent(t -> 
            System.out.println("Largest transaction: " + t.getAmount())
        );
        
        // Custom collector
        String summary = transactions.stream()
            .collect(Collector.of(
                StringBuilder::new,
                (sb, t) -> sb.append(t.toString()).append("\n"),
                StringBuilder::append,
                StringBuilder::toString
            ));
    }
}
```
## Optional Class
Optional is a container object that may or may not contain a non-null value. It helps eliminate NullPointerException and makes null-handling explicit:
```java 
// Creating Optional
Optional<String> empty = Optional.empty();
Optional<String> present = Optional.of("Hello");
Optional<String> nullable = Optional.ofNullable(getString());  // May be null

// Checking presence
if (present.isPresent()) {
    System.out.println(present.get());
}

// Better approach - functional style
present.ifPresent(System.out::println);

// Java 9+ enhancements
present.ifPresentOrElse(
    value -> System.out.println("Value: " + value),
    () -> System.out.println("No value present")
);

// Getting values
String value1 = present.get();  // Throws NoSuchElementException if empty
String value2 = empty.orElse("Default");
String value3 = empty.orElseGet(() -> generateDefault());
String value4 = empty.orElseThrow(() -> new IllegalStateException("No value"));

// Transforming Optional
Optional<Integer> length = present.map(String::length);
Optional<String> upperCase = present.map(String::toUpperCase);

// FlatMap for nested Optionals
Optional<Optional<String>> nested = Optional.of(Optional.of("Hello"));
Optional<String> flattened = nested.flatMap(opt -> opt);

// Filtering
Optional<String> filtered = present
    .filter(s -> s.length() > 3)
    .filter(s -> s.startsWith("H"));

// Combining operations
String result = Optional.ofNullable(getString())
    .map(String::trim)
    .filter(s -> !s.isEmpty())
    .map(String::toUpperCase)
    .orElse("DEFAULT");

// Stream of Optionals (Java 9+)
List<Optional<String>> optionals = Arrays.asList(
    Optional.of("a"),
    Optional.empty(),
    Optional.of("b")
);

List<String> values = optionals.stream()
    .flatMap(Optional::stream)  // Java 9+
    .collect(Collectors.toList());  // [a, b]
```
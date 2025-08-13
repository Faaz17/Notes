# Chapter 13: Collections Framework
## Understanding Collections
The Collections Framework is a unified architecture for representing and manipulating collections of objects. It provides interfaces, implementations, and algorithms to work with groups of objects efficiently.

Collections are more flexible than arrays:

* Dynamic size
* Built-in methods for common operations
* Type-safe with generics
* Different implementations for different needs

## Collection Hierarchy
The main interfaces in the Collections Framework:

* Collection: Root interface

* List: Ordered, allows duplicates (ArrayList, LinkedList)
* Set: No duplicates (HashSet, TreeSet)
* Queue: FIFO operations (LinkedList, PriorityQueue)


* Map: Key-value pairs (HashMap, TreeMap)

## List Interface
Lists maintain insertion order and allow duplicate elements. Elements can be accessed by index.
## ArrayList
Dynamic array implementation. Best for frequent access and iteration.
```java 
import java.util.ArrayList;

ArrayList<String> fruits = new ArrayList<>();

// Adding elements
fruits.add("Apple");
fruits.add("Banana");
fruits.add(1, "Orange");  // Insert at index

// Accessing elements
String first = fruits.get(0);
fruits.set(0, "Mango");  // Replace element

// Removing elements
fruits.remove("Banana");
fruits.remove(0);  // Remove by index

// Other operations
int size = fruits.size();
boolean hasApple = fruits.contains("Apple");
fruits.clear();  // Remove all

// Iteration
for (String fruit : fruits) {
    System.out.println(fruit);
}
```
## LinkedList
Doubly-linked list implementation. Best for frequent insertion/deletion.
```java 
import java.util.LinkedList;

LinkedList<Integer> numbers = new LinkedList<>();

// List operations
numbers.add(10);
numbers.addFirst(5);
numbers.addLast(15);
numbers.removeFirst();
numbers.removeLast();

// Can also be used as Queue
numbers.offer(20);      // Add to end
numbers.poll();         // Remove from front
numbers.peek();         // View first element
```
## Set Interface
Sets don't allow duplicate elements. Different implementations provide different ordering guarantees.
### HashSet
No ordering, fastest performance.
```java 
import java.util.HashSet;

HashSet<Integer> numbers = new HashSet<>();
numbers.add(10);
numbers.add(20);
numbers.add(10);  // Ignored, duplicate

System.out.println(numbers.size());  // 2

// Set operations
HashSet<Integer> set1 = new HashSet<>();
HashSet<Integer> set2 = new HashSet<>();

set1.addAll(set2);     // Union
set1.retainAll(set2);  // Intersection
set1.removeAll(set2);  // Difference
```
### TreeSet
Sorted set using natural ordering or comparator.
```java 
import java.util.TreeSet;

TreeSet<String> words = new TreeSet<>();
words.add("banana");
words.add("apple");
words.add("cherry");

// Automatically sorted: [apple, banana, cherry]
for (String word : words) {
    System.out.println(word);
}
```
## Map Interface
Maps store key-value pairs. Keys must be unique, values can be duplicated.
### HashMap
Hash table implementation. No ordering, allows one null key.
```java 
import java.util.HashMap;

HashMap<String, Integer> scores = new HashMap<>();

// Adding entries
scores.put("Alice", 95);
scores.put("Bob", 87);
scores.put("Charlie", 92);

// Accessing values
int aliceScore = scores.get("Alice");
int defaultScore = scores.getOrDefault("David", 0);

// Removing entries
scores.remove("Bob");

// Checking existence
boolean hasAlice = scores.containsKey("Alice");
boolean has95 = scores.containsValue(95);

// Iteration
for (Map.Entry<String, Integer> entry : scores.entrySet()) {
    System.out.println(entry.getKey() + ": " + entry.getValue());
}
```
### TreeMap
Sorted map based on keys.
```java 
import java.util.TreeMap;

TreeMap<String, String> countries = new TreeMap<>();
countries.put("US", "United States");
countries.put("IN", "India");
countries.put("UK", "United Kingdom");

// Automatically sorted by keys
String firstKey = countries.firstKey();    // "IN"
String lastKey = countries.lastKey();      // "US"
```
## Queue Interface
Queues typically follow FIFO (First In First Out) order.
```java 
import java.util.Queue;
import java.util.LinkedList;
import java.util.PriorityQueue;

// Using LinkedList as Queue
Queue<String> queue = new LinkedList<>();
queue.offer("First");    // Add to end
queue.offer("Second");
String first = queue.poll();  // Remove and return first
String peek = queue.peek();   // View first without removing

// PriorityQueue - elements are ordered by priority
PriorityQueue<Integer> pq = new PriorityQueue<>();
pq.offer(5);
pq.offer(1);
pq.offer(3);
// Removes in order: 1, 3, 5
```

## Iterators
Iterators provide a way to traverse collections:
```java 
ArrayList<String> list = new ArrayList<>();
list.add("A");
list.add("B");
list.add("C");

// Using Iterator
Iterator<String> it = list.iterator();
while (it.hasNext()) {
    String element = it.next();
    if (element.equals("B")) {
        it.remove();  // Safe removal during iteration
    }
}

// Using ListIterator (bidirectional)
ListIterator<String> lit = list.listIterator();
while (lit.hasNext()) {
    System.out.println(lit.next());
}
while (lit.hasPrevious()) {
    System.out.println(lit.previous());
}
```
## Collections Utility Class
The Collections class provides static methods for common operations:
```java 
import java.util.Collections;

ArrayList<Integer> numbers = new ArrayList<>();
numbers.add(5);
numbers.add(2);
numbers.add(8);

// Sorting
Collections.sort(numbers);                    // Natural order
Collections.sort(numbers, Collections.reverseOrder());  // Reverse

// Searching (list must be sorted)
int index = Collections.binarySearch(numbers, 5);

// Other operations
Collections.reverse(numbers);                 // Reverse list
Collections.shuffle(numbers);                 // Random order
int max = Collections.max(numbers);
int min = Collections.min(numbers);
Collections.fill(numbers, 0);                // Fill with value
```
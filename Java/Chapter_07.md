# Chapter 7: Object-Oriented Programming - Classes and Objects
## Understanding OOP Concepts
Object-Oriented Programming (OOP) is a programming paradigm that organizes code around objects rather than functions and logic. An object is a self-contained entity that contains both data (attributes) and methods (behaviors) that operate on that data.

Think of OOP as modeling real-world entities. For example, a car has attributes (color, model, speed) and behaviors (start, stop, accelerate). In OOP, we create a Car class as a blueprint, and then create individual car objects from that blueprint.

The main principles of OOP are:

1. Encapsulation: Bundling data and methods together, hiding internal details
2. Inheritance: Creating new classes based on existing ones
3. Polymorphism: Objects of different types responding to the same interface
4. Abstraction: Hiding complex implementation details

## Classes and Objects
A class is a blueprint or template that defines the structure and behavior of objects. It doesn't consume memory until objects are created from it.

An object is an instance of a class. It's a concrete entity created from the class blueprint that occupies memory and has a state.
```java
// Class definition
public class Student {
    // Instance variables (attributes/fields)
    String name;
    int age;
    String studentId;
    
    // Method (behavior)
    public void study() {
        System.out.println(name + " is studying");
    }
    
    public void displayInfo() {
        System.out.println("Name: " + name);
        System.out.println("Age: " + age);
        System.out.println("ID: " + studentId);
    }
}

// Creating and using objects
public class Main {
    public static void main(String[] args) {
        // Creating objects
        Student student1 = new Student();
        Student student2 = new Student();
        
        // Setting values
        student1.name = "Alice";
        student1.age = 20;
        student1.studentId = "S001";
        
        student2.name = "Bob";
        student2.age = 21;
        student2.studentId = "S002";
        
        // Calling methods
        student1.study();       // Alice is studying
        student2.displayInfo();
    }
}
```
## Constructors
A constructor is a special method that initializes objects. It has the same name as the class and no return type. Constructors are called automatically when an object is created using the new keyword.

Types of constructors:

1. Default Constructor: No parameters, provided by Java if no constructor is defined
2. Parameterized Constructor: Accepts parameters to initialize object with specific values
3. Copy Constructor: Creates a new object as a copy of an existing object
```java 
public class Book {
    private String title;
    private String author;
    private int pages;
    
    // Default constructor
    public Book() {
        title = "Unknown";
        author = "Unknown";
        pages = 0;
    }
    
    // Parameterized constructor
    public Book(String title, String author, int pages) {
        this.title = title;    // 'this' refers to current object
        this.author = author;
        this.pages = pages;
    }
    
    // Copy constructor
    public Book(Book other) {
        this.title = other.title;
        this.author = other.author;
        this.pages = other.pages;
    }
    
    // Constructor overloading
    public Book(String title) {
        this(title, "Unknown", 0);  // Calling another constructor
    }
}
```
## The 'this' Keyword
The this keyword refers to the current object. It's used to:

1. Distinguish between instance variables and parameters with the same name
2. Call another constructor in the same class
3. Pass the current object as a parameter
4. Return the current object
```java 
public class Person {
    private String name;
    private int age;
    
    public Person(String name, int age) {
        this.name = name;  // this.name = instance variable, name = parameter
        this.age = age;
    }
    
    public Person setName(String name) {
        this.name = name;
        return this;  // Returns current object for method chaining
    }
    
    public void compareTo(Person other) {
        if (this.age > other.age) {
            System.out.println(this.name + " is older");
        }
    }
}
```
## Access Modifiers
Access modifiers control the visibility and accessibility of classes, methods, and variables. Java has four access levels:

1. private: Accessible only within the same class
2. default (no modifier): Accessible within the same package
3. protected: Accessible within the same package and subclasses
4. public: Accessible from anywhere
```java 
public class Employee {
    private int salary;        // Only within Employee class
    String department;         // Within same package
    protected int id;          // Package + subclasses
    public String name;        // Accessible everywhere
    
    // Private method
    private void calculateBonus() {
        // Internal calculation
    }
    
    // Public method
    public void displayInfo() {
        calculateBonus();  // Can call private method internally
        System.out.println("Name: " + name);
    }
}
```
## Static Members
Static members belong to the class itself rather than any instance. They're shared among all objects of the class and can be accessed without creating an object.

Static members include:

* Static variables (class variables)
* Static methods
* Static blocks
* Static nested classes
```java 
public class Counter {
    private static int totalCount = 0;  // Shared among all instances
    private int instanceCount = 0;      // Each object has its own
    
    public Counter() {
        totalCount++;    // Increments for every object created
        instanceCount++;
    }
    
    // Static method
    public static int getTotalCount() {
        return totalCount;
        // Cannot access instanceCount here
    }
    
    // Instance method
    public int getInstanceCount() {
        return instanceCount;
        // Can access both static and instance members
    }
    
    // Static block - runs once when class is first loaded
    static {
        System.out.println("Counter class loaded");
        totalCount = 0;
    }
}

// Usage
Counter c1 = new Counter();
Counter c2 = new Counter();
System.out.println(Counter.getTotalCount());  // 2
```
## Nested Classes
Java allows you to define a class within another class. These are called nested classes and are of four types:

1. Static Nested Class: Doesn't need reference to outer class
2. Inner Class: Non-static nested class, needs outer class instance
3. Local Inner Class: Defined inside a method
4. Anonymous Inner Class: No name, declared and instantiated at once
```java 
public class Outer {
    private int outerField = 10;
    private static int staticField = 20;
    
    // Static nested class
    static class StaticNested {
        void display() {
            System.out.println("Static field: " + staticField);
            // Cannot access outerField
        }
    }
    
    // Inner class
    class Inner {
        void display() {
            System.out.println("Outer field: " + outerField);
            System.out.println("Static field: " + staticField);
        }
    }
    
    // Method with local inner class
    void method() {
        class LocalInner {
            void print() {
                System.out.println("Local inner class");
            }
        }
        LocalInner local = new LocalInner();
        local.print();
    }
}
```
## 
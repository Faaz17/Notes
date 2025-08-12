# Chapter 8: Inheritance
## Understanding Inheritance
Inheritance is a mechanism where a new class is derived from an existing class. The new class (child/subclass) inherits properties and methods from the existing class (parent/superclass). This promotes code reuse and establishes an "is-a" relationship between classes.

For example, if you have a Vehicle class with common properties like speed and color, you can create specific classes like Car and Bike that inherit from Vehicle and add their own specific features.

## Benefits of inheritance:

1. Code Reusability: Avoid duplicating code
2. Method Overriding: Customize behavior in child classes
3. Polymorphism: Treat different objects uniformly
4. Extensibility: Add new features without modifying existing code

## Basic Inheritance
In Java, inheritance is achieved using the extends keyword. A class can only extend one class (single inheritance), but inheritance can be multi-level (A → B → C).
```java 
// Parent class (superclass)
public class Animal {
    protected String name;
    protected int age;
    
    public void eat() {
        System.out.println(name + " is eating");
    }
    
    public void sleep() {
        System.out.println(name + " is sleeping");
    }
}

// Child class (subclass)
public class Dog extends Animal {
    private String breed;
    
    public void bark() {
        System.out.println(name + " is barking");
    }
    
    public void wagTail() {
        System.out.println("Wagging tail");
    }
}

// Usage
Dog myDog = new Dog();
myDog.name = "Buddy";      // Inherited field
myDog.eat();               // Inherited method
myDog.bark();              // Own method
```
## Method Overriding
Method overriding allows a subclass to provide a specific implementation of a method that is already defined in its parent class. The method in the child class must have the same signature as in the parent class.

### Rules for overriding:

1. Method signature must be exactly the same
2. Access modifier can be same or less restrictive
3. Return type must be same or subtype (covariant return)
4. Cannot override static, final, or private methods
```java 
public class Vehicle {
    public void start() {
        System.out.println("Vehicle starting");
    }
    
    public void stop() {
        System.out.println("Vehicle stopping");
    }
}

public class Car extends Vehicle {
    @Override  // Optional but recommended annotation
    public void start() {
        System.out.println("Car starting with key");
    }
    
    // Adding new behavior while keeping parent behavior
    @Override
    public void stop() {
        System.out.println("Applying brakes");
        super.stop();  // Call parent's version
    }
}
```
## The super Keyword
The super keyword refers to the immediate parent class. It's used to:

1. Call parent class constructor
2. Access parent class methods
3. Access parent class fields (if they're hidden)
```java 
public class Employee {
    protected String name;
    protected double salary;
    
    public Employee(String name, double salary) {
        this.name = name;
        this.salary = salary;
    }
    
    public void work() {
        System.out.println(name + " is working");
    }
}

public class Manager extends Employee {
    private String department;
    
    public Manager(String name, double salary, String department) {
        super(name, salary);  // Must be first statement
        this.department = department;
    }
    
    @Override
    public void work() {
        super.work();  // Call parent's work method
        System.out.println("Managing " + department + " department");
    }
    
    public void displayInfo() {
        System.out.println("Name: " + super.name);  // Accessing parent field
        System.out.println("Department: " + department);
    }
}
```
## Constructor Chaining
When a subclass object is created, constructors are called in a specific order:

1. Parent class constructor (implicitly or explicitly)
2. Child class constructor

If you don't explicitly call a parent constructor using super(), Java automatically calls the parent's no-argument constructor.
```java 
public class Person {
    protected String name;
    
    public Person() {
        System.out.println("Person constructor");
    }
    
    public Person(String name) {
        this.name = name;
        System.out.println("Person constructor with name");
    }
}

public class Student extends Person {
    private String studentId;
    
    public Student() {
        // Implicit super() call
        System.out.println("Student constructor");
    }
    
    public Student(String name, String studentId) {
        super(name);  // Explicit call to parent constructor
        this.studentId = studentId;
        System.out.println("Student constructor with parameters");
    }
}

// Creating object
Student s = new Student("Alice", "S001");
// Output:
// Person constructor with name
// Student constructor with parameters
```
## Types of Inheritance
Java supports several types of inheritance:

1. Single Inheritance: One class extends another class
2. Multilevel Inheritance: Chain of inheritance (A → B → C)
3. Hierarchical Inheritance: Multiple classes extend same parent

Java does NOT support multiple inheritance with classes (one class extending multiple classes) to avoid the diamond problem. However, multiple inheritance is possible with interfaces.
```java 
// Multilevel inheritance
class Animal { }
class Mammal extends Animal { }
class Dog extends Mammal { }

// Hierarchical inheritance
class Shape { }
class Circle extends Shape { }
class Rectangle extends Shape { }
class Triangle extends Shape { }
```
## The final Keyword with Inheritance
The final keyword can be used to restrict inheritance:

* final class: Cannot be extended
* final method: Cannot be overridden
* final variable: Cannot be reassigned (constant)
```java 
// Final class - cannot be inherited
public final class MathConstants {
    public static final double PI = 3.14159;
    public static final double E = 2.71828;
}

// Class with final method
public class Parent {
    public final void importantMethod() {
        // This method cannot be overridden
        System.out.println("Critical implementation");
    }
    
    public void normalMethod() {
        // Can be overridden
    }
}
```
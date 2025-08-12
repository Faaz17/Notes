# Chapter 10: Abstraction
## Understanding Abstraction
Abstraction is the process of hiding implementation details and showing only essential features to the user. It's about creating a simple interface that hides the complex underlying implementation. Think of it like a TV remote - you press buttons to change channels without knowing the internal circuitry.

Abstraction in Java is achieved through:

1. Abstract Classes: Partial abstraction
2. Interfaces: Complete abstraction

## Abstract Classes
An abstract class is a class that cannot be instantiated and may contain abstract methods (methods without implementation). It's used as a base class for other classes to extend.
Key points:

* Cannot create objects of abstract classes
* Can have both abstract and concrete methods
* Can have constructors and instance variables
* A class extending an abstract class must implement all abstract methods

```java 
// Abstract class
public abstract class Vehicle {
    protected String brand;
    protected int year;
    
    // Constructor
    public Vehicle(String brand, int year) {
        this.brand = brand;
        this.year = year;
    }
    
    // Abstract methods (no implementation)
    public abstract void start();
    public abstract void stop();
    
    // Concrete method (with implementation)
    public void displayInfo() {
        System.out.println("Brand: " + brand);
        System.out.println("Year: " + year);
    }
}

// Concrete class extending abstract class
public class Car extends Vehicle {
    private int doors;
    
    public Car(String brand, int year, int doors) {
        super(brand, year);
        this.doors = doors;
    }
    
    @Override
    public void start() {
        System.out.println("Car starting with key");
    }
    
    @Override
    public void stop() {
        System.out.println("Car stopping with brakes");
    }
}
```
## Interfaces
An interface is a completely abstract type that defines a contract for classes to follow. It specifies what methods a class must have, but not how they should be implemented.

Key points:

* All methods are public abstract by default (before Java 8)
* All variables are public static final by default
* A class can implement multiple interfaces
* Interfaces can extend other interfaces
```java 
// Interface definition
public interface Playable {
    void play();    // public abstract by default
    void pause();
    void stop();
}

public interface Recordable {
    void record();
    void saveRecording(String filename);
}

// Class implementing interfaces
public class MediaPlayer implements Playable, Recordable {
    @Override
    public void play() {
        System.out.println("Playing media");
    }
    
    @Override
    public void pause() {
        System.out.println("Pausing media");
    }
    
    @Override
    public void stop() {
        System.out.println("Stopping media");
    }
    
    @Override
    public void record() {
        System.out.println("Recording");
    }
    
    @Override
    public void saveRecording(String filename) {
        System.out.println("Saving to " + filename);
    }
}
```
## Java 8+ Interface Features
Java 8 introduced default and static methods in interfaces, allowing interfaces to have method implementations.
```java 
public interface SmartDevice {
    // Abstract method
    void turnOn();
    void turnOff();
    
    // Default method (Java 8+)
    default void reset() {
        System.out.println("Resetting device");
        turnOff();
        turnOn();
    }
    
    // Static method (Java 8+)
    static void showVersion() {
        System.out.println("SmartDevice v1.0");
    }
    
    // Private method (Java 9+)
    private void log(String message) {
        System.out.println("Log: " + message);
    }
}
```
## Abstract Class vs Interface
When to use Abstract Class:

* When you want to share code among closely related classes
* When you need non-static or non-final fields
* When you need constructors
* When you need private or protected members

When to use Interface:

* When unrelated classes need to implement a contract
* When you need multiple inheritance
* When you want to specify behavior without implementation
* When creating a plugin architecture
```java 
// Abstract class example - related classes
public abstract class Animal {
    protected String name;
    
    public Animal(String name) {
        this.name = name;
    }
    
    public abstract void makeSound();
}

// Interface example - unrelated classes
public interface Flyable {
    void fly();
}

class Bird extends Animal implements Flyable {
    public Bird(String name) {
        super(name);
    }
    
    public void makeSound() {
        System.out.println("Chirp");
    }
    
    public void fly() {
        System.out.println("Flying in the sky");
    }
}

class Airplane implements Flyable {
    public void fly() {
        System.out.println("Flying with engines");
    }
}
class Bird extends Animal implements Flyable {
public Bird(String name) {
super(name);
}
public void makeSound() {
    System.out.println("Chirp");
}

public void fly() {
    System.out.println("Flying in the sky");
}
}
class Airplane implements Flyable {
public void fly() {
System.out.println("Flying with engines");
}
}
```
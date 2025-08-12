# Chapter 9: Polymorphism
## Understanding Polymorphism
Polymorphism means "many forms". It's the ability of an object to take on multiple forms. In Java, polymorphism allows us to perform a single action in different ways. It lets us define methods in the parent class and override them in child classes to provide different implementations.

### There are two types of polymorphism in Java:

1. Compile-time Polymorphism (Static): Method overloading
2. Runtime Polymorphism (Dynamic): Method overriding

Polymorphism is one of the core concepts of OOP and provides flexibility and code reusability.

## Compile-time Polymorphism (Method Overloading)
This is achieved through method overloading, where multiple methods have the same name but different parameters. The compiler determines which method to call based on the method signature at compile time.
```java 
public class Calculator {
    // Same method name, different parameters
    public int add(int a, int b) {
        return a + b;
    }
    
    public int add(int a, int b, int c) {
        return a + b + c;
    }
    
    public double add(double a, double b) {
        return a + b;
    }
    
    // Constructor overloading
    public Calculator() {
        System.out.println("Default calculator");
    }
    
    public Calculator(String type) {
        System.out.println(type + " calculator");
    }
}
```
## Runtime Polymorphism (Method Overriding)
This occurs when a child class provides a specific implementation of a method that is already defined in the parent class. The method to be called is determined at runtime based on the actual object type, not the reference type.

This is the true power of polymorphism - the ability to treat different objects uniformly through a common interface.
```java 
// Parent class
public class Shape {
    public void draw() {
        System.out.println("Drawing a shape");
    }
    
    public double area() {
        return 0;
    }
}

// Child classes
public class Circle extends Shape {
    private double radius;
    
    public Circle(double radius) {
        this.radius = radius;
    }
    
    @Override
    public void draw() {
        System.out.println("Drawing a circle");
    }
    
    @Override
    public double area() {
        return Math.PI * radius * radius;
    }
}

public class Rectangle extends Shape {
    private double width, height;
    
    public Rectangle(double width, double height) {
        this.width = width;
        this.height = height;
    }
    
    @Override
    public void draw() {
        System.out.println("Drawing a rectangle");
    }
    
    @Override
    public double area() {
        return width * height;
    }
}

// Polymorphic usage
Shape shape1 = new Circle(5);
Shape shape2 = new Rectangle(4, 6);

shape1.draw();  // Drawing a circle
shape2.draw();  // Drawing a rectangle

// Polymorphic array
Shape[] shapes = {
    new Circle(3),
    new Rectangle(2, 4),
    new Circle(5)
};

for (Shape shape : shapes) {
    shape.draw();
    System.out.println("Area: " + shape.area());
}
```
## Upcasting and Downcasting
**Upcasting:** Converting a subclass reference to a superclass reference. This happens automatically and is always safe.

**Downcasting:** Converting a superclass reference to a subclass reference. This requires explicit casting and can cause ClassCastException if the object isn't actually of the target type.
```java 
// Upcasting (automatic)
Dog dog = new Dog();
Animal animal = dog;  // Upcasting

// Downcasting (explicit)
Animal animalRef = new Dog();
Dog dogRef = (Dog) animalRef;  // Downcasting

// Safe downcasting with instanceof
if (animalRef instanceof Dog) {
    Dog safeDog = (Dog) animalRef;
    safeDog.bark();  // Dog-specific method
}

// Polymorphic method
public void handleAnimal(Animal animal) {
    animal.eat();  // Common method
    
    if (animal instanceof Dog) {
        Dog dog = (Dog) animal;
        dog.bark();  // Dog-specific
    } else if (animal instanceof Cat) {
        Cat cat = (Cat) animal;
        cat.meow();  // Cat-specific
    }
}
```
## The instanceof Operator
The instanceof operator tests whether an object is an instance of a specific class or interface. It's commonly used before downcasting to avoid ClassCastException.
```java 
public void processShape(Shape shape) {
    shape.draw();  // Polymorphic call
    
    if (shape instanceof Circle) {
        Circle circle = (Circle) shape;
        System.out.println("This is a circle with radius");
    } else if (shape instanceof Rectangle) {
        Rectangle rect = (Rectangle) shape;
        System.out.println("This is a rectangle");
    }
}

// With Java 14+ pattern matching
if (shape instanceof Circle circle) {
    // circle is already cast and available
    System.out.println("Circle radius: " + circle.getRadius());
}
```
## Polymorphism with Interfaces
Interfaces provide another way to achieve polymorphism. A class can implement multiple interfaces, allowing for more flexible polymorphic behavior.
```java 
interface Drawable {
    void draw();
}

interface Calculable {
    double calculate();
}

class Triangle implements Drawable, Calculable {
    public void draw() {
        System.out.println("Drawing triangle");
    }
    
    public double calculate() {
        // Calculate area
        return 0.5 * base * height;
    }
}

// Polymorphic usage
Drawable d = new Triangle();
d.draw();

Calculable c = new Triangle();
double result = c.calculate();
```
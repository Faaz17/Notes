# Chapter 6: Methods
## Understanding Methods
A method is a block of code that performs a specific task and can be reused multiple times. Methods are fundamental to structured programming as they help break down complex problems into smaller, manageable pieces. Think of methods as mini-programs within your program that you can call whenever you need their functionality.

## Methods provide several benefits:
1. Code Reusability: Write once, use many times
2. Modularity: Break complex problems into smaller parts
3. Maintainability: Changes in one place affect all uses
4. Abstraction: Hide implementation details
5. Testing: Test individual units of functionality

## Method Structure and Syntax
A method in Java has several components that define its behavior and how it can be used:
```java 
// Method syntax
accessModifier returnType methodName(parameterList) {
    // Method body
    // Statements
    return value;  // If returnType is not void
}

// Example
public int addNumbers(int a, int b) {
    int sum = a + b;
    return sum;
}
```
## Components explained:

* Access Modifier: Controls who can call the method (public, private, protected, default)
* Return Type: Type of value the method returns (int, String, void, etc.)
* Method Name: Identifier following camelCase convention
* Parameters: Input values the method accepts
* Method Body: The code that executes when method is called
* Return Statement: Sends a value back to the caller (not needed for void methods)

## Types of Methods
### Void Methods
These methods perform actions but don't return any value. Use void when the method only needs to do something, not calculate and return a result.
```java
Components explained:

Access Modifier: Controls who can call the method (public, private, protected, default)
Return Type: Type of value the method returns (int, String, void, etc.)
Method Name: Identifier following camelCase convention
Parameters: Input values the method accepts
Method Body: The code that executes when method is called
Return Statement: Sends a value back to the caller (not needed for void methods)

Types of Methods
Void Methods
These methods perform actions but don't return any value. Use void when the method only needs to do something, not calculate and return a result.
```

### Return Methods
These methods compute and return a value. The return type must match the type of value being returned.
```java 
public int calculateSum(int a, int b) {
    return a + b;
}

public boolean isEven(int number) {
    return number % 2 == 0;
}

public String getFullName(String firstName, String lastName) {
    return firstName + " " + lastName;
}
```
### Static vs Instance Methods
Static methods belong to the class itself, not to any specific instance. They can be called without creating an object of the class. They cannot access instance variables or methods directly.
Instance methods belong to objects and can access both instance and static members.
```java 
Static vs Instance Methods
Static methods belong to the class itself, not to any specific instance. They can be called without creating an object of the class. They cannot access instance variables or methods directly.
Instance methods belong to objects and can access both instance and static members.
```
## Method Parameters
Parameters are variables that accept values when a method is called. Java uses pass-by-value, meaning it passes a copy of the value, not the original variable itself.

For primitives, the actual value is copied. For objects, the reference value (memory address) is copied, but both references point to the same object.
```java 
// Primitive parameters - changes don't affect original
public void modifyPrimitive(int x) {
    x = 100;  // Only changes local copy
}

// Object parameters - can modify object contents
public void modifyArray(int[] arr) {
    arr[0] = 100;  // Changes original array
}

// But reassigning doesn't affect original reference
public void reassignArray(int[] arr) {
    arr = new int[]{1, 2, 3};  // Doesn't affect original
}

// Example usage
int num = 50;
modifyPrimitive(num);
System.out.println(num);  // Still 50

int[] numbers = {10, 20, 30};
modifyArray(numbers);
System.out.println(numbers[0]);  // Now 100
```
## Method Overloading
Method overloading allows multiple methods with the same name but different parameters. The compiler determines which method to call based on the arguments provided.
Methods are overloaded when they have:

Different number of parameters
Different types of parameters
Different order of parameters

Return type alone is not sufficient for overloading.
```java 
Method Overloading
Method overloading allows multiple methods with the same name but different parameters. The compiler determines which method to call based on the arguments provided.
Methods are overloaded when they have:

Different number of parameters
Different types of parameters
Different order of parameters

Return type alone is not sufficient for overloading.
```
## Variable Arguments (Varargs)
Varargs allow methods to accept a variable number of arguments of the same type. The varargs parameter must be the last parameter and there can be only one varargs parameter.

```java 
// Varargs method
public int sum(int... numbers) {
    int total = 0;
    for (int num : numbers) {
        total += num;
    }
    return total;
}

// Usage
int result1 = sum(1, 2);           // 3
int result2 = sum(1, 2, 3, 4, 5);  // 15
int result3 = sum();               // 0

// Combining regular and varargs parameters
public void printInfo(String name, int... scores) {
    System.out.println("Name: " + name);
    for (int score : scores) {
        System.out.println("Score: " + score);
    }
}
```
## Recursion
Recursion is when a method calls itself. Every recursive method needs:

1. A base case (stopping condition)
2. A recursive case (method calls itself with modified parameters)
```java 
Recursion
Recursion is when a method calls itself. Every recursive method needs:

A base case (stopping condition)
A recursive case (method calls itself with modified parameters)
```
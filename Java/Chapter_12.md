# Chapter 12: Exception Handling
## Understanding Exceptions
An exception is an event that disrupts the normal flow of a program's execution. It's an object that represents an error or unexpected behavior. Rather than letting the program crash, Java provides a mechanism to handle these exceptions gracefully.

Exceptions can occur due to:

* Programming errors (null pointer, array index out of bounds)
* User input errors (invalid data format)
* Device errors (file not found, network connection lost)
* Physical limitations (out of memory, disk full)

## Exception Hierarchy
All exceptions in Java inherit from the Throwable class:

Throwable

* Error: Serious problems that applications shouldn't handle (OutOfMemoryError)
* Exception: Problems that applications should handle

* Checked Exceptions: Must be handled or declared (IOException, SQLException)
* Unchecked Exceptions: Runtime exceptions (NullPointerException, ArrayIndexOutOfBoundsException)

Try-Catch-Finally
The try-catch-finally block is used to handle exceptions:
```java
public class ExceptionExample {
    public static void main(String[] args) {
        try {
            // Code that might throw an exception
            int result = 10 / 0;
            System.out.println("Result: " + result);
        } catch (ArithmeticException e) {
            // Handle specific exception
            System.out.println("Cannot divide by zero!");
            System.out.println("Error: " + e.getMessage());
        } catch (Exception e) {
            // Handle any other exception
            System.out.println("An error occurred: " + e);
        } finally {
            // Always executes, regardless of exception
            System.out.println("Cleanup code here");
        }
    }
}
```
## Multiple Catch Blocks
You can have multiple catch blocks to handle different types of exceptions. Always catch more specific exceptions before general ones.
```java 
public void processFile(String filename) {
    try {
        FileReader file = new FileReader(filename);
        int[] array = new int[5];
        array[10] = 50;  // ArrayIndexOutOfBoundsException
    } catch (FileNotFoundException e) {
        System.out.println("File not found: " + filename);
    } catch (ArrayIndexOutOfBoundsException e) {
        System.out.println("Array index error");
    } catch (Exception e) {
        System.out.println("General error: " + e);
    }
}

// Multi-catch (Java 7+)
try {
    // Code that might throw multiple exceptions
} catch (IOException | SQLException e) {
    System.out.println("IO or SQL error: " + e);
}
```
## Throwing Exceptions
You can throw exceptions explicitly using the throw keyword. Use throws in method signature to declare that a method might throw an exception.
```java 
public class Validator {
    // Method that throws checked exception
    public void validateAge(int age) throws InvalidAgeException {
        if (age < 0 || age > 150) {
            throw new InvalidAgeException("Age must be between 0 and 150");
        }
    }
    
    // Method that throws unchecked exception
    public void checkNotNull(Object obj) {
        if (obj == null) {
            throw new NullPointerException("Object cannot be null");
        }
    }
    
    // Using the methods
    public static void main(String[] args) {
        Validator validator = new Validator();
        
        try {
            validator.validateAge(200);
        } catch (InvalidAgeException e) {
            System.out.println("Validation failed: " + e.getMessage());
        }
    }
}
```
## Custom Exceptions
You can create your own exception classes for specific error conditions in your application:
```java 
// Custom checked exception
public class InsufficientFundsException extends Exception {
    private double amount;
    
    public InsufficientFundsException(double amount) {
        super("Insufficient funds for withdrawal of " + amount);
        this.amount = amount;
    }
    
    public double getAmount() {
        return amount;
    }
}

// Custom unchecked exception
public class InvalidOperationException extends RuntimeException {
    public InvalidOperationException(String message) {
        super(message);
    }
}

// Using custom exception
public class BankAccount {
    private double balance;
    
    public void withdraw(double amount) throws InsufficientFundsException {
        if (amount > balance) {
            throw new InsufficientFundsException(amount);
        }
        balance -= amount;
    }
}
```
## Try-with-Resources
Java 7 introduced try-with-resources for automatic resource management. Resources that implement AutoCloseable are automatically closed
```java 
// Old way
FileReader fr = null;
try {
    fr = new FileReader("file.txt");
    // Read file
} catch (IOException e) {
    e.printStackTrace();
} finally {
    if (fr != null) {
        try {
            fr.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}

// Try-with-resources (Java 7+)
try (FileReader fr = new FileReader("file.txt");
     BufferedReader br = new BufferedReader(fr)) {
    // Read file
    // Resources automatically closed
} catch (IOException e) {
    e.printStackTrace();
}
```
# Chapter 11: Encapsulation
## Understanding Encapsulation
Encapsulation is the process of bundling data (variables) and methods that operate on that data within a single unit (class), and restricting direct access to some of the object's components. It's like a protective wrapper that prevents external code from directly accessing the internal state of an object.

The main purpose of encapsulation is to:

1. Hide the internal implementation details
2. Protect data from unauthorized access
3. Maintain data integrity through controlled access
4. Make code more maintainable and flexible

## Implementing Encapsulation
Encapsulation is achieved by:

1. Declaring variables as private
2. Providing public getter and setter methods to access and modify the variables
3. Adding validation logic in setters to ensure data integrity
```java 
public class BankAccount {
    // Private fields - cannot be accessed directly from outside
    private String accountNumber;
    private String accountHolder;
    private double balance;
    
    // Constructor
    public BankAccount(String accountNumber, String accountHolder) {
        this.accountNumber = accountNumber;
        this.accountHolder = accountHolder;
        this.balance = 0.0;
    }
    
    // Getter methods - provide read access
    public String getAccountNumber() {
        return accountNumber;
    }
    
    public String getAccountHolder() {
        return accountHolder;
    }
    
    public double getBalance() {
        return balance;
    }
    
    // Setter methods - provide write access with validation
    public void deposit(double amount) {
        if (amount > 0) {
            balance += amount;
            System.out.println("Deposited: $" + amount);
        } else {
            System.out.println("Invalid deposit amount");
        }
    }
    
    public boolean withdraw(double amount) {
        if (amount > 0 && amount <= balance) {
            balance -= amount;
            System.out.println("Withdrawn: $" + amount);
            return true;
        } else {
            System.out.println("Invalid withdrawal amount");
            return false;
        }
    }
}
```
## Benefits of Encapsulation

1. Data Hiding: Internal representation is hidden from the outside world
2. Increased Flexibility: Can change internal implementation without affecting other classes
3. Reusability: Encapsulated code can be reused easily
4. Testing: Encapsulated code is easier to unit test
```java 
public class Person {
    private String name;
    private int age;
    
    // Validation in setter
    public void setAge(int age) {
        if (age >= 0 && age <= 150) {
            this.age = age;
        } else {
            throw new IllegalArgumentException("Invalid age");
        }
    }
    
    // Computed property
    public boolean isAdult() {
        return age >= 18;
    }
    
    // Formatted output
    public String getFormattedName() {
        return name.toUpperCase();
    }
}
```
## Read-Only and Write-Only Classes
Sometimes you want to create classes where data can only be read or only written:
```java 
// Read-only class - immutable
public class ImmutablePoint {
    private final int x;
    private final int y;
    
    public ImmutablePoint(int x, int y) {
        this.x = x;
        this.y = y;
    }
    
    // Only getters, no setters
    public int getX() { return x; }
    public int getY() { return y; }
}

// Write-only class (rare)
public class Logger {
    private StringBuilder log = new StringBuilder();
    
    // Only setter, no getter
    public void addLog(String message) {
        log.append(message).append("\n");
    }
    
    // Process internally without exposing data
    private void saveToFile() {
        // Save log to file
    }
}
```
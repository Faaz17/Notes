# Chapter 4: Control Flow Statements
## Understanding Control Flow
By default, programs execute statements sequentially from top to bottom. Control flow statements allow you to alter this flow based on conditions, repeat code multiple times, or jump to different parts of your program. They're what make programs dynamic and responsive to different situations.
### Control flow can be categorized into:

1. Selection statements (if, switch) - Choose which code to execute
2. Iteration statements (for, while, do-while) - Repeat code multiple times
3. Jump statements (break, continue, return) - Transfer control to another part

## if Statements
The if statement is the most fundamental decision-making construct. It executes code only when a specified condition is true. You can chain multiple conditions using else if and provide a default action with else.

The condition inside if must evaluate to a boolean value. Unlike some languages, Java doesn't treat numbers as boolean (0 is not false, non-zero is not true).
```java 
int score = 85;

// Simple if
if (score >= 60) {
    System.out.println("Passed");
}

// if-else
if (score >= 60) {
    System.out.println("Passed");
} else {
    System.out.println("Failed");
}

// if-else-if ladder
if (score >= 90) {
    System.out.println("Grade: A");
} else if (score >= 80) {
    System.out.println("Grade: B");
} else if (score >= 70) {
    System.out.println("Grade: C");
} else {
    System.out.println("Grade: F");
}

// Nested if
if (score >= 60) {
    if (score >= 90) {
        System.out.println("Passed with distinction");
    } else {
        System.out.println("Passed");
    }
}
```
## switch Statement
The switch statement provides a cleaner way to compare a single variable against multiple specific values. It's more readable than multiple if-else statements when checking for exact matches.
Switch works with: byte, short, int, char, String (Java 7+), enums, and their wrapper classes. Each case must be a compile-time constant.
```java 
int day = 3;

// Traditional switch
switch (day) {
    case 1:
        System.out.println("Monday");
        break;  // Important! Prevents fall-through
    case 2:
        System.out.println("Tuesday");
        break;
    case 3:
        System.out.println("Wednesday");
        break;
    default:
        System.out.println("Invalid day");
}

// Multiple cases (fall-through)
char grade = 'B';
switch (grade) {
    case 'A':
    case 'B':
        System.out.println("Excellent");
        break;
    case 'C':
        System.out.println("Good");
        break;
    default:
        System.out.println("Need improvement");
}

// Enhanced switch (Java 14+)
String dayType = switch (day) {
    case 1, 2, 3, 4, 5 -> "Weekday";
    case 6, 7 -> "Weekend";
    default -> "Invalid";
};
```
## for Loop
The for loop is used when you know in advance how many times you want to repeat something. It combines initialization, condition checking, and update in a compact syntax.

Structure: for (initialization; condition; update) { body }

The enhanced for loop (for-each) is specifically designed for iterating through arrays and collections
```java 
// Standard for loop
for (int i = 0; i < 5; i++) {
    System.out.println("Iteration " + i);
}

// Multiple variables
for (int i = 0, j = 10; i < j; i++, j--) {
    System.out.println("i=" + i + ", j=" + j);
}

// Enhanced for loop (for-each)
int[] numbers = {10, 20, 30, 40, 50};
for (int num : numbers) {
    System.out.println(num);  // Can't modify array elements
}

// Nested loops
for (int i = 1; i <= 3; i++) {
    for (int j = 1; j <= 3; j++) {
        System.out.print(i * j + " ");
    }
    System.out.println();
}
```
## while Loop
The while loop repeats code as long as a condition is true. Use it when you don't know exactly how many iterations you need, but you know the stopping condition.

The condition is checked before each iteration, so the loop might not execute at all if the condition is initially false.
```java 
// Basic while loop
int count = 0;
while (count < 5) {
    System.out.println("Count: " + count);
    count++;
}

// Using while for unknown iterations
Scanner scanner = new Scanner(System.in);
String input = "";
while (!input.equals("quit")) {
    System.out.print("Enter command: ");
    input = scanner.nextLine();
}
```

## do-while Loop
The do-while loop is similar to while, but the condition is checked after each iteration. This guarantees the loop body executes at least once, even if the condition is initially false.

Use do-while when you need the code to run at least once, like displaying a menu.
```java 
// Basic do-while
int i = 0;
do {
    System.out.println(i);
    i++;
} while (i < 5);

// Executes once even with false condition
int x = 10;
do {
    System.out.println("This prints once");
} while (x < 5);  // False, but body already executed

// Menu example
int choice;
do {
    System.out.println("1. Option 1");
    System.out.println("2. Option 2");
    System.out.println("3. Exit");
    choice = scanner.nextInt();
} while (choice != 3);
```
## Jump Statements
Jump statements transfer control to another part of the program.
break: Exits the innermost loop or switch statement immediately.
continue: Skips the rest of the current iteration and continues with the next iteration.
return: Exits the current method and optionally returns a value.
```java 
// break - exit loop
for (int i = 0; i < 10; i++) {
    if (i == 5) {
        break;  // Exit when i is 5
    }
    System.out.print(i + " ");  // Prints: 0 1 2 3 4
}

// continue - skip iteration
for (int i = 0; i < 10; i++) {
    if (i % 2 == 0) {
        continue;  // Skip even numbers
    }
    System.out.print(i + " ");  // Prints: 1 3 5 7 9
}

// Labeled break (for nested loops)
outer:
for (int i = 0; i < 3; i++) {
    for (int j = 0; j < 3; j++) {
        if (i == 1 && j == 1) {
            break outer;  // Exits both loops
        }
        System.out.println(i + "," + j);
    }
}
```
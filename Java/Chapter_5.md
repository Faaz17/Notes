# Chapter 5: Arrays
## Understanding Arrays
An array is a container object that holds a fixed number of values of a single type. 

The key characteristics of arrays in Java are:

1. Fixed size - once created, the size cannot change
2. Homogeneous - all elements must be of the same type
3. Indexed - elements are accessed by their position (index)
4. Zero-based - first element is at index 0
5. Arrays are objects - they're stored in heap memory

Arrays provide a way to store multiple values in a single variable, instead of declaring separate variables for each value. They're particularly useful when you need to process a collection of similar items.

## Array Declaration and Creation
In Java, array declaration and creation are separate steps. Declaration tells Java the type of array, while creation actually allocates memory for it.
```java
// Declaration
int[] numbers;        // Preferred style
int numbers2[];      // C-style (valid but not recommended)

// Creation
numbers = new int[5];  // Creates array of 5 integers

// Declaration and creation together
int[] scores = new int[10];

// Declaration, creation, and initialization
int[] marks = {85, 90, 78, 92, 88};  // Size is 5

// Alternative syntax
int[] ages = new int[]{25, 30, 35};  // Size is 3
```

## Default Values
When you create an array without initializing values, Java provides default values based on the array type:

* Numeric types (byte, short, int, long): 0
* Floating-point (float, double): 0.0
* char: '\u0000' (null character)
* boolean: false
* Object references: null
```java
Default Values
When you create an array without initializing values, Java provides default values based on the array type:

Numeric types (byte, short, int, long): 0
Floating-point (float, double): 0.0
char: '\u0000' (null character)
boolean: false
Object references: null
```

## Accessing Array Elements
Array elements are accessed using square brackets with the index. Trying to access an index outside the array bounds throws ArrayIndexOutOfBoundsException.
```java 
int[] arr = {10, 20, 30, 40, 50};

// Accessing elements
int first = arr[0];   // 10
int last = arr[4];    // 50
// int invalid = arr[5];  // Runtime error!

// Modifying elements
arr[0] = 15;          // Changes first element to 15

// Array length property
int size = arr.length;  // 5 (note: it's a property, not a method)
```
## Traversing Arrays
There are several ways to iterate through array elements:
```java
int[] numbers = {10, 20, 30, 40, 50};

// Traditional for loop (when you need index)
for (int i = 0; i < numbers.length; i++) {
    System.out.println("Index " + i + ": " + numbers[i]);
}

// Enhanced for loop (when you only need values)
for (int num : numbers) {
    System.out.println(num);
}

// While loop
int i = 0;
while (i < numbers.length) {
    System.out.println(numbers[i]);
    i++;
}
```
## Multi-dimensional Arrays
Arrays can have multiple dimensions. A 2D array is essentially an array of arrays. Java allows jagged arrays (rows of different lengths).
```java 
// 2D array (matrix)
int[][] matrix = new int[3][4];  // 3 rows, 4 columns

// Initialization
int[][] grid = {
    {1, 2, 3},
    {4, 5, 6},
    {7, 8, 9}
};

// Accessing elements
int element = grid[1][2];  // 6 (row 1, column 2)

// Traversing 2D array
for (int row = 0; row < grid.length; row++) {
    for (int col = 0; col < grid[row].length; col++) {
        System.out.print(grid[row][col] + " ");
    }
    System.out.println();
}

// Jagged array (different row lengths)
int[][] jagged = {
    {1, 2},
    {3, 4, 5, 6},
    {7}
};
```
## Array Manipulation with Arrays Class
The java.util.Arrays class provides useful methods for array manipulation:
```java 
import java.util.Arrays;

int[] arr = {5, 2, 8, 1, 9};

// Sorting
Arrays.sort(arr);  // Modifies original: {1, 2, 5, 8, 9}

// Searching (array must be sorted)
int index = Arrays.binarySearch(arr, 5);  // Returns 2

// Copying
int[] copy = Arrays.copyOf(arr, arr.length);
int[] partial = Arrays.copyOfRange(arr, 1, 4);  // {2, 5, 8}

// Filling
int[] filled = new int[5];
Arrays.fill(filled, 10);  // {10, 10, 10, 10, 10}

// Comparing
boolean equal = Arrays.equals(arr, copy);  // true

// Converting to string
System.out.println(Arrays.toString(arr));  // [1, 2, 5, 8, 9]
```
## Common Array Operations
```java
// Finding maximum/minimum
int[] nums = {45, 23, 78, 12, 90, 34};
int max = nums[0];
int min = nums[0];
for (int num : nums) {
    if (num > max) max = num;
    if (num < min) min = num;
}

// Sum and average
int sum = 0;
for (int num : nums) {
    sum += num;
}
double average = (double) sum / nums.length;

// Reversing an array
for (int i = 0; i < nums.length / 2; i++) {
    int temp = nums[i];
    nums[i] = nums[nums.length - 1 - i];
    nums[nums.length - 1 - i] = temp;
}
```
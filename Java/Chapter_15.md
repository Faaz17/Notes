# Chapter 15: File Handling
## Understanding File I/O
File handling allows programs to read from and write to files on the disk. Java provides several classes for file operations, with improvements in each version. The modern approach uses the java.nio.file package (New I/O), but understanding the older java.io package is also important.

File operations include:

* Creating, deleting, and renaming files
* Reading from and writing to files
* Working with directories
* Handling file attributes and permissions

## File Class
The File class represents a file or directory path. It doesn't contain the file data but provides methods to work with files and directories.
```java 
import java.io.File;

File file = new File("example.txt");
File dir = new File("/path/to/directory");

// File operations
boolean exists = file.exists();
boolean created = file.createNewFile();  // Creates if doesn't exist
boolean deleted = file.delete();
boolean renamed = file.renameTo(new File("newname.txt"));

// File information
String name = file.getName();
String path = file.getAbsolutePath();
long size = file.length();  // Size in bytes
long modified = file.lastModified();  // Timestamp
boolean isFile = file.isFile();
boolean isDir = file.isDirectory();
boolean canRead = file.canRead();
boolean canWrite = file.canWrite();

// Directory operations
File folder = new File("myFolder");
boolean created = folder.mkdir();  // Create directory
String[] files = folder.list();    // List file names
File[] fileObjects = folder.listFiles();  // List File objects
```
## Writing to Files
Using FileWriter
```java 
import java.io.FileWriter;
import java.io.BufferedWriter;

// Basic FileWriter
try (FileWriter writer = new FileWriter("output.txt")) {
    writer.write("Hello World\n");
    writer.write("Java File Handling");
} catch (IOException e) {
    e.printStackTrace();
}

// BufferedWriter for better performance
try (BufferedWriter bw = new BufferedWriter(new FileWriter("output.txt", true))) {  // true for append
    bw.write("Line 1");
    bw.newLine();
    bw.write("Line 2");
} catch (IOException e) {
    e.printStackTrace();
}
```
## Using PrintWriter
```java 
import java.io.PrintWriter;

try (PrintWriter pw = new PrintWriter("output.txt")) {
    pw.println("Hello World");
    pw.printf("Number: %d%n", 42);
    pw.print("No newline");
} catch (IOException e) {
    e.printStackTrace();
}
```
## Reading from Files
Using FileReader and BufferedReader
```java 
import java.io.FileReader;
import java.io.BufferedReader;

// Reading character by character
try (FileReader reader = new FileReader("input.txt")) {
    int ch;
    while ((ch = reader.read()) != -1) {
        System.out.print((char) ch);
    }
} catch (IOException e) {
    e.printStackTrace();
}

// Reading line by line (more efficient)
try (BufferedReader br = new BufferedReader(new FileReader("input.txt"))) {
    String line;
    while ((line = br.readLine()) != null) {
        System.out.println(line);
    }
} catch (IOException e) {
    e.printStackTrace();
}
```
## Using Scanner
```java 
import java.util.Scanner;
import java.io.File;

try (Scanner scanner = new Scanner(new File("input.txt"))) {
    while (scanner.hasNextLine()) {
        String line = scanner.nextLine();
        System.out.println(line);
    }
    
    // Reading different types
    // scanner.nextInt();
    // scanner.nextDouble();
    // scanner.next();  // Next word
} catch (FileNotFoundException e) {
    e.printStackTrace();
}
```
## Binary Files
For non-text files, use byte streams:
```java
import java.io.*;

// Writing binary data
try (FileOutputStream fos = new FileOutputStream("data.dat");
     DataOutputStream dos = new DataOutputStream(fos)) {
    dos.writeInt(42);
    dos.writeDouble(3.14);
    dos.writeUTF("Hello");
} catch (IOException e) {
    e.printStackTrace();
}

// Reading binary data
try (FileInputStream fis = new FileInputStream("data.dat");
     DataInputStream dis = new DataInputStream(fis)) {
    int num = dis.readInt();
    double pi = dis.readDouble();
    String str = dis.readUTF();
} catch (IOException e) {
    e.printStackTrace();
}
```
## Modern File I/O (NIO.2)
Java 7 introduced the java.nio.file package with improved file handling:
```java 
import java.nio.file.*;
import java.nio.charset.StandardCharsets;

Path path = Paths.get("example.txt");

// Reading all lines
List<String> lines = Files.readAllLines(path);

// Reading all content
String content = Files.readString(path);  // Java 11+

// Writing
Files.write(path, "Hello World".getBytes());
Files.writeString(path, "Hello World");  // Java 11+

// Writing lines
List<String> linesToWrite = Arrays.asList("Line 1", "Line 2", "Line 3");
Files.write(path, linesToWrite);

// File operations
Files.copy(source, target, StandardCopyOption.REPLACE_EXISTING);
Files.move(source, target);
Files.delete(path);
boolean exists = Files.exists(path);

// Working with directories
Files.createDirectory(Paths.get("newDir"));
Files.createDirectories(Paths.get("path/to/dir"));  // Creates parent dirs

// Walking directory tree
Files.walk(Paths.get("."))
    .filter(Files::isRegularFile)
    .forEach(System.out::println);
```
# Chapter 16: Multithreading
## Understanding Threads
A thread is a lightweight subprocess, the smallest unit of processing. Multithreading allows concurrent execution of two or more parts of a program for maximum CPU utilization. Each thread runs parallel to others but shares the same memory space.

Java is multi-threaded by design. Even a simple Java program has multiple threads:

* Main thread (executes main method)
* Garbage collector thread
* Other system threads

## Benefits of multithreading:

* Better CPU utilization
* Improved application responsiveness
* Parallel processing
* Better system resource usage

## Creating Threads
There are two ways to create threads in Java:
1. Extending Thread Class
```java 
class MyThread extends Thread {
    public void run() {
        // Code that runs in the thread
        for (int i = 1; i <= 5; i++) {
            System.out.println(Thread.currentThread().getName() + ": " + i);
            try {
                Thread.sleep(1000);  // Pause for 1 second
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}

// Using the thread
MyThread t1 = new MyThread();
MyThread t2 = new MyThread();
t1.start();  // Don't call run() directly!
t2.start();
```
2. Implementing Runnable Interface (Preferred)
```java 
class MyRunnable implements Runnable {
    public void run() {
        for (int i = 1; i <= 5; i++) {
            System.out.println(Thread.currentThread().getName() + ": " + i);
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}

// Using the runnable
Thread t1 = new Thread(new MyRunnable());
Thread t2 = new Thread(new MyRunnable());
t1.start();
t2.start();

// Using lambda (Java 8+)
Thread t3 = new Thread(() -> {
    System.out.println("Lambda thread running");
});
t3.start();
```
## Thread Lifecycle
A thread goes through various states:

1. New: Thread created but not started
2. Runnable: Thread is ready to run
3. Running: Thread is executing
4. Blocked/Waiting: Thread is temporarily inactive
5. Terminated: Thread has completed execution
```java
Thread t = new Thread(() -> {
    System.out.println("Running");
});

System.out.println(t.getState());  // NEW
t.start();
System.out.println(t.getState());  // RUNNABLE
Thread.sleep(100);
System.out.println(t.getState());  // TERMINATED
```
# Java Thread Class Methods

This note explains important constructors and methods of the Java `Thread` class in a very simple way.

## What is `Thread` class?

In Java, the `Thread` class is used to create and manage threads.

A **thread** is a small worker inside a program that can execute a task independently.

Simple analogy:

```text
Java Program = Company
Thread       = Worker
run()        = Work assigned to worker
start()      = Tell worker to start working
```

The `Thread` class is present in the `java.lang` package, so we do not need to import it manually.

---

## Basic Declaration

```java
public class Thread extends Object implements Runnable
```

Meaning:

- `Thread` is a class.
- It implements the `Runnable` interface.
- Because of `Runnable`, the `Thread` class has the `run()` method.

---

# Thread Class Constructors

Constructors are used to create `Thread` objects.

## Common Constructors

| Constructor | Simple Meaning |
|---|---|
| `Thread()` | Creates a new thread object without giving any task or name. |
| `Thread(Runnable target)` | Creates a thread object and gives it a task to execute. |
| `Thread(String name)` | Creates a thread object with a specific name. |
| `Thread(Runnable target, String name)` | Creates a thread with both task and name. |
| `Thread(ThreadGroup group, Runnable target)` | Creates a thread inside a specific thread group with a task. |
| `Thread(ThreadGroup group, String name)` | Creates a thread inside a specific thread group with a name. |
| `Thread(ThreadGroup group, Runnable target, String name)` | Creates a thread inside a group with task and name. |
| `Thread(ThreadGroup group, Runnable target, String name, long stackSize)` | Creates a thread with group, task, name, and stack size. Usually not needed for beginners. |

---

## Simple Constructor Example

```java
class MyTask implements Runnable {
    public void run() {
        System.out.println("Task is running");
    }
}

public class Main {
    public static void main(String[] args) {
        Thread t1 = new Thread(new MyTask());
        t1.start();
    }
}
```

Explanation:

```text
new MyTask()       = task
new Thread(task)   = worker created for that task
t1.start()         = worker starts the task
```

---

# Important Methods of Thread Class

We can divide Thread class methods into simple categories:

1. Basic methods
2. Naming methods
3. Daemon thread methods
4. Priority methods
5. Execution control methods
6. Interrupting methods
7. Deprecated methods

---

# 1. Basic Methods

## `run()`

The `run()` method contains the actual task of the thread.

```java
public void run()
```

Example:

```java
class MyThread extends Thread {
    public void run() {
        System.out.println("Thread task is running");
    }
}
```

Simple meaning:

```text
run() = task/job of the thread
```

Important point:

If we call `run()` directly, it behaves like a normal method call. It does not start a new thread.

---

## `start()`

The `start()` method starts a new thread.

```java
public synchronized void start()
```

Example:

```java
class MyThread extends Thread {
    public void run() {
        System.out.println("Thread is running");
    }
}

public class Main {
    public static void main(String[] args) {
        MyThread t1 = new MyThread();
        t1.start();
    }
}
```

Simple meaning:

```text
start() = start a new worker
```

Important point:

```text
start() internally calls run()
```

So always use `start()` when you want a new thread.

---

## `currentThread()`

The `currentThread()` method returns the currently running thread.

```java
public static native Thread currentThread()
```

Example:

```java
public class Main {
    public static void main(String[] args) {
        Thread t = Thread.currentThread();
        System.out.println(t.getName());
    }
}
```

Output:

```text
main
```

Simple meaning:

```text
currentThread() = Which thread is running right now?
```

---

## `isAlive()`

The `isAlive()` method checks whether a thread is still alive or running.

```java
public final native boolean isAlive()
```

Example:

```java
class MyThread extends Thread {
    public void run() {
        System.out.println("Thread running");
    }
}

public class Main {
    public static void main(String[] args) {
        MyThread t1 = new MyThread();
        System.out.println(t1.isAlive());

        t1.start();
        System.out.println(t1.isAlive());
    }
}
```

Possible output:

```text
false
true
```

Simple meaning:

```text
isAlive() = Is this thread still active?
```

---

# 2. Naming Methods

Naming methods are used to give names to threads and get thread names.

---

## `getName()`

The `getName()` method returns the name of a thread.

```java
public final String getName()
```

Example:

```java
public class Main {
    public static void main(String[] args) {
        Thread t1 = new Thread();
        System.out.println(t1.getName());
    }
}
```

Possible output:

```text
Thread-0
```

Simple meaning:

```text
getName() = Tell me the thread name
```

---

## `setName(String name)`

The `setName()` method changes the name of a thread.

```java
public final synchronized void setName(String name)
```

Example:

```java
public class Main {
    public static void main(String[] args) {
        Thread t1 = new Thread();
        t1.setName("Worker-1");
        System.out.println(t1.getName());
    }
}
```

Output:

```text
Worker-1
```

Simple meaning:

```text
setName() = Give a name to the thread
```

---

# 3. Daemon Thread Methods

A **daemon thread** is a background helper thread.

Simple analogy:

```text
Normal thread = Main worker
Daemon thread = Background helper
```

Example of background helper work:

- Garbage collection
- Monitoring
- Background cleanup

Important:

When all normal user threads finish, daemon threads do not keep the JVM alive.

---

## `isDaemon()`

The `isDaemon()` method checks whether a thread is daemon or not.

```java
public final boolean isDaemon()
```

Example:

```java
public class Main {
    public static void main(String[] args) {
        Thread t1 = new Thread();
        System.out.println(t1.isDaemon());
    }
}
```

Output:

```text
false
```

Simple meaning:

```text
isDaemon() = Is this thread a background helper thread?
```

---

## `setDaemon(boolean on)`

The `setDaemon()` method marks a thread as daemon or non-daemon.

```java
public final void setDaemon(boolean on)
```

Example:

```java
public class Main {
    public static void main(String[] args) {
        Thread t1 = new Thread();
        t1.setDaemon(true);
        t1.start();
    }
}
```

Important point:

`setDaemon(true)` must be called before `start()`.

Simple meaning:

```text
setDaemon(true) = Make this thread a background helper
```

---

# 4. Priority-Based Methods

Every thread has a priority.

Thread priority tells the thread scheduler which thread is more important, but it does not guarantee exact execution order.

Java thread priority range:

```text
Minimum priority = 1
Normal priority  = 5
Maximum priority = 10
```

Java constants:

```java
Thread.MIN_PRIORITY   // 1
Thread.NORM_PRIORITY  // 5
Thread.MAX_PRIORITY   // 10
```

---

## `getPriority()`

The `getPriority()` method returns the priority of a thread.

```java
public final int getPriority()
```

Example:

```java
public class Main {
    public static void main(String[] args) {
        Thread t1 = new Thread();
        System.out.println(t1.getPriority());
    }
}
```

Output:

```text
5
```

Simple meaning:

```text
getPriority() = Tell me the thread priority
```

---

## `setPriority(int newPriority)`

The `setPriority()` method changes the priority of a thread.

```java
public final void setPriority(int newPriority)
```

Example:

```java
public class Main {
    public static void main(String[] args) {
        Thread t1 = new Thread();
        t1.setPriority(Thread.MAX_PRIORITY);
        System.out.println(t1.getPriority());
    }
}
```

Output:

```text
10
```

Simple meaning:

```text
setPriority() = Change importance level of thread
```

Important point:

Priority gives a hint to the scheduler, but it does not guarantee that the highest priority thread will always run first.

---

# 5. Methods That Pause or Control Thread Execution

These methods are used to pause, wait, or allow other threads to run.

---

## `sleep(long millis)`

The `sleep()` method pauses the current thread for a given time.

```java
public static native void sleep(long millis) throws InterruptedException
```

Example:

```java
public class Main {
    public static void main(String[] args) throws InterruptedException {
        System.out.println("Before sleep");
        Thread.sleep(1000);
        System.out.println("After sleep");
    }
}
```

Output:

```text
Before sleep
After sleep
```

There will be a delay of around 1 second between both lines.

Simple meaning:

```text
sleep() = Take rest for some time
```

---

## `yield()`

The `yield()` method gives a hint to the thread scheduler that the current thread is ready to pause and allow another thread to run.

```java
public static native void yield()
```

Simple meaning:

```text
yield() = I can wait; let another thread run if needed
```

Important point:

`yield()` does not guarantee that another thread will definitely run immediately.

---

## `join()`

The `join()` method makes one thread wait until another thread finishes.

```java
public final void join() throws InterruptedException
```

Example:

```java
class MyThread extends Thread {
    public void run() {
        for (int i = 1; i <= 3; i++) {
            System.out.println(i);
        }
    }
}

public class Main {
    public static void main(String[] args) throws InterruptedException {
        MyThread t1 = new MyThread();
        t1.start();

        t1.join();

        System.out.println("Main thread continues after t1 finishes");
    }
}
```

Simple meaning:

```text
join() = Wait until this thread completes
```

Analogy:

```text
Main thread says: I will wait until Worker-1 completes the job.
```

---

# 6. Interrupting Methods

Interrupting means requesting a thread to stop waiting, sleeping, or blocking.

Important:

`interrupt()` does not forcefully kill a thread. It sends a request/signal to interrupt the thread.

---

## `interrupt()`

The `interrupt()` method interrupts a thread.

```java
public void interrupt()
```

Example:

```java
class MyThread extends Thread {
    public void run() {
        try {
            Thread.sleep(5000);
        } catch (InterruptedException e) {
            System.out.println("Thread was interrupted");
        }
    }
}

public class Main {
    public static void main(String[] args) {
        MyThread t1 = new MyThread();
        t1.start();
        t1.interrupt();
    }
}
```

Simple meaning:

```text
interrupt() = Send interruption request to thread
```

---

## `isInterrupted()`

The `isInterrupted()` method checks whether a specific thread has been interrupted.

```java
public boolean isInterrupted()
```

Simple meaning:

```text
isInterrupted() = Has this thread received an interrupt request?
```

---

## `interrupted()`

The `interrupted()` method checks whether the current thread has been interrupted.

```java
public static boolean interrupted()
```

Important point:

`interrupted()` checks the current thread and clears the interrupted status after checking.

Simple meaning:

```text
interrupted() = Is current thread interrupted? Then clear the interrupt flag.
```

---

# 7. Deprecated Methods

Some old Thread methods are deprecated because they are unsafe.

Avoid using these methods in modern Java programs.

| Deprecated Method | Why Avoid It? |
|---|---|
| `stop()` | It can stop a thread suddenly and leave shared data in an inconsistent state. |
| `suspend()` | It can pause a thread while holding a lock, causing deadlock. |
| `resume()` | It was used with `suspend()`, so it can also lead to unsafe behavior. |
| `destroy()` | It was never properly implemented and should not be used. |

Simple meaning:

```text
Deprecated = Old method, not recommended to use
```

Instead of these methods, use safer techniques like:

- `interrupt()`
- boolean flag
- ExecutorService shutdown methods

---

# Quick Summary Table

| Method | Category | Simple Meaning |
|---|---|---|
| `run()` | Basic | Contains the task of the thread. |
| `start()` | Basic | Starts a new thread and internally calls `run()`. |
| `currentThread()` | Basic | Returns the currently running thread. |
| `isAlive()` | Basic | Checks whether the thread is still active. |
| `getName()` | Naming | Gets the name of the thread. |
| `setName()` | Naming | Changes the name of the thread. |
| `isDaemon()` | Daemon | Checks whether the thread is daemon. |
| `setDaemon()` | Daemon | Marks a thread as daemon or non-daemon. |
| `getPriority()` | Priority | Gets the priority of the thread. |
| `setPriority()` | Priority | Changes the priority of the thread. |
| `sleep()` | Execution control | Pauses the current thread for some time. |
| `yield()` | Execution control | Gives chance to other threads. |
| `join()` | Execution control | Waits for another thread to finish. |
| `interrupt()` | Interrupting | Sends interrupt request to a thread. |
| `isInterrupted()` | Interrupting | Checks interrupt status of a specific thread. |
| `interrupted()` | Interrupting | Checks and clears interrupt status of current thread. |

---

# Most Important Points to Remember

## 1. Use `start()`, not `run()`

```java
t1.start();
```

This creates a new thread.

```java
t1.run();
```

This behaves like a normal method call.

---

## 2. One thread can be started only once

```java
Thread t1 = new Thread();
t1.start();
t1.start(); // Error: IllegalThreadStateException
```

A thread object cannot be restarted after it has already been started.

---

## 3. Thread execution order is not fixed

If multiple threads are running, output order may change every time.

Why?

Because the thread scheduler decides which thread gets CPU time.

---

## 4. Avoid deprecated methods

Do not use:

```java
stop();
suspend();
resume();
destroy();
```

Use safe approaches like `interrupt()` instead.

---

# Small Complete Example

```java
class MyThread extends Thread {
    public void run() {
        System.out.println("Running thread: " + Thread.currentThread().getName());
    }
}

public class Main {
    public static void main(String[] args) throws InterruptedException {
        MyThread t1 = new MyThread();

        t1.setName("Worker-1");
        t1.setPriority(Thread.MAX_PRIORITY);

        System.out.println("Before start: " + t1.isAlive());

        t1.start();
        t1.join();

        System.out.println("After finish: " + t1.isAlive());
    }
}
```

Possible output:

```text
Before start: false
Running thread: Worker-1
After finish: false
```

---

# Final Simple Definition

The `Thread` class provides methods to create, start, pause, name, prioritize, join, interrupt, and check the status of threads.

In simple words:

```text
Thread methods help us control workers inside a Java program.
```

---
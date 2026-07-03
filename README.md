# Multithreading in Java — Complete Notes

---

## 📑 Index

1. [Introduction to Multithreading](#1-introduction-to-multithreading)
2. [Difference Between Process vs Thread](#2-difference-between-process-and-thread)

---

## 1. Introduction to Multithreading

### 🤔 Before We Begin — What Problem Are We Solving?

Imagine you're at a **restaurant** — but there's only **one waiter** for the entire place. 

He takes an order from Table 1, walks to the kitchen, waits for the food to be ready, brings it back, and *only then* moves to Table 2. Meanwhile, Tables 3 through 10 are just sitting there — hungry and frustrated.

> Now imagine the restaurant hires **five waiters**. Suddenly, multiple tables are being served at the same time. Orders are taken in parallel, food arrives faster, and customers are happy.

**That's essentially what multithreading does for your Java programs.**

> Your program is the restaurant. The waiters are **threads**. And the customers? They're the **tasks** your program needs to get done.

---

### 📖 What is a Program, a Process, and a Thread?

Before jumping into multithreading, let's nail down three terms you'll hear constantly. Think of them as three levels of zoom:

#### Program

A program is a set of instructions written in a programming language, stored as a file on disk. It is a passive entity — it doesn't do anything until it is executed. For example, a .java file or a compiled .class file sitting on your hard drive is a program.

```
📁 MyApp.java  ←  This is a program (just a file, doing nothing)
```

#### Process

A process is a running instance of a program. When you **run** that program, the operating system loads it into memory and starts executing it. Now it becomes a **process**

Think of it like this: the recipe book is still on the shelf, but now a **chef has opened it and started cooking**. That active cooking session is a process.

> A process gets its own chunk of memory, its own resources, and is managed by the operating system.

#### Thread

A thread is the smallest unit of execution within a process that the CPU can schedule and run.

> If a process is a **kitchen**, then a thread is a **single chef** working in that kitchen. You can have one chef (single-threaded) or many chefs working together (multithreaded).

```
┌──────────────────────────────────────────────┐
│                  PROCESS                     │
│          (Your running Java app)             │
│                                              │
│   ┌──────────┐  ┌──────────┐  ┌──────────┐  │
│   │ Thread 1 │  │ Thread 2 │  │ Thread 3 │  │
│   │ (Chef 1) │  │ (Chef 2) │  │ (Chef 3) │  │
│   └──────────┘  └──────────┘  └──────────┘  │
│                                              │
│        Shared Memory (Shared Kitchen)        │
└──────────────────────────────────────────────┘
```

> **Key insight:** All threads within a process **share the same memory space** (like chefs sharing the same kitchen counter and ingredients). This is what makes threads lightweight and fast — but it's also what makes multithreading tricky (more on that later).

#### Quick Comparison

| Concept     | Real-Life Analogy           | Key Characteristic                                  |
|-------------|-----------------------------|------------------------------------------------------|
| **Program** | A recipe book on a shelf    | Static, just instructions, not running               |
| **Process** | A kitchen actively cooking  | Running instance, has its own memory & resources     |
| **Thread**  | A single chef in the kitchen| A flow of execution inside a process, shares memory  |

---

### 🧵 What Exactly is a Thread?

Let's zoom in a little more.

A **thread** is simply a **sequence of instructions** that can be executed independently. When your Java program starts, the JVM automatically creates one thread for you — the **main thread**. This is the thread that begins executing your `main()` method.

```java
public class HelloWorld {
    public static void main(String[] args) {
        // This code runs on the "main" thread
        System.out.println("Hello from the main thread!");
    }
}
```

Even this simple program uses a thread — you just never had to think about it before!

---

### 🚶 Single-Threaded Execution — One Thing at a Time

In a **single-threaded** program, tasks are done **one after another**, like a single queue at a billing counter. Task 2 can't start until Task 1 finishes. Task 3 waits for Task 2. And so on.

```
Single-Threaded Execution (Sequential):

Time ──────────────────────────────────────────►

 ┌────────────────┐┌────────────────┐┌────────────────┐
 │   Task A       ││   Task B       ││   Task C       │
 │  (Download     ││  (Process      ││  (Save to      │
 │   a file)      ││   data)        ││   database)    │
 └────────────────┘└────────────────┘└────────────────┘

 ◄──── 5 sec ────►◄──── 3 sec ────►◄──── 2 sec ────►

                Total time: 10 seconds
```

Everything happens in a straight line. Simple? Yes. Efficient? **Not always.**

#### The Problems with Single-Threaded Execution

1. **Wasted time during waiting:** If Task A is downloading a file from the internet, the CPU is mostly just *waiting* for the network. It could be doing something useful instead.

2. **Frozen user interfaces:** Ever clicked a button in an app and it froze? That's because the single thread was busy doing something else (like loading data) and couldn't respond to your click.

3. **No use of modern hardware:** Your computer likely has 4, 8, or even 16 CPU cores. A single-threaded program uses only **one** of them. The rest sit idle — like having a 16-lane highway but only allowing one car on it.

```
Your CPU cores in a single-threaded program:

   Core 1:  ████████████████████  (doing all the work)
   Core 2:  ░░░░░░░░░░░░░░░░░░░░  (idle... 😴)
   Core 3:  ░░░░░░░░░░░░░░░░░░░░  (idle... 😴)
   Core 4:  ░░░░░░░░░░░░░░░░░░░░  (idle... 😴)

   █ = Working    ░ = Idle
```

---

### 🚀 Multithreaded Execution — Doing Multiple Things at Once

**Multithreading** means having **multiple threads** running within the same process, so multiple tasks can make progress at the same time.

> **Definition :**

**Multithreading** is a programming concept where **multiple threads run within the same process,** allowing multiple tasks to make progress simultaneously. It improves application performance by utilizing CPU resources more efficiently, keeps the UI responsive, and allows better use of multi-core processors. Java has built-in support for multithreading from the very beginning.

Going back to our earlier example:

```
Multithreaded Execution (Concurrent / Parallel):

Time ──────────────────────────────────────────►

 Thread 1: ┌────────────────┐
            │   Task A       │
            │  (Download)    │
            └────────────────┘
 Thread 2: ┌────────────────┐
            │   Task B       │
            │  (Process)     │
            └────────────────┘
 Thread 3: ┌────────────────┐
            │   Task C       │
            │  (Save to DB)  │
            └────────────────┘

            ◄──── 5 sec ────►

          Total time: ~5 seconds (instead of 10!)
```

Now the same work gets done in roughly **half the time** because tasks run in parallel, not one after another.

```
Your CPU cores in a multithreaded program:

   Core 1:  ████████████████████  (Task A)
   Core 2:  ████████████████████  (Task B)
   Core 3:  ████████████████████  (Task C)
   Core 4:  ░░░░░░░░░░░░░░░░░░░░  (available for other work)

   Now we're actually using our hardware! 💪
```

---

### ⏱️ What is Context Switching?

We mentioned **context switching** above. Let's make sure it's crystal clear.

When a CPU core has to switch from running one thread to another, it needs to:

1. **Save** the current state of Thread A (where it was in the code, what variables it was using, etc.)
2. **Load** the saved state of Thread B
3. **Resume** Thread B from where it left off

This entire process is called a **context switch**.

```
Context Switching Visualized:

CPU Core 1:
    ┌──────┐ save ┌──────┐ save ┌──────┐ save ┌──────┐
    │  T1  │──────│  T2  │──────│  T3  │──────│  T1  │──►
    └──────┘ load └──────┘ load └──────┘ load └──────┘
    
    T1, T2, T3 = Different Threads
    
    Each "save/load" is a context switch.
    It's fast, but not free — there's a small overhead.
```

> **Think of it like this:** You're reading a novel, but every 5 minutes someone hands you a different book. You have to bookmark your page, pick up the new book, find your bookmark in *that* book, and continue reading. That "bookmark, switch, find" process is the overhead of context switching.

---

### 🌍 Where is Multithreading Used in the Real World?

Multithreading isn't just a theoretical concept — it's **everywhere** in the software you use daily:

| Application             | How Multithreading is Used                                                                 |
|--------------------------|--------------------------------------------------------------------------------------------|
| **🌐 Web Browsers**     | One thread renders the page, another loads images, another handles your clicks              |
| **🎮 Video Games**      | One thread for graphics rendering, one for physics, one for audio, one for user input       |
| **💻 IDEs (IntelliJ)**  | One thread for the editor, another for code analysis, another for building in the background |
| **🎵 Music Players**    | One thread plays the audio, another updates the progress bar, another loads the next song   |
| **🌐 Web Servers**      | Each incoming request from a user is handled by a separate thread                           |
| **💬 Chat Apps**        | One thread listens for incoming messages, another sends your messages, another updates the UI|
| **📱 Android Apps**      | The "main" thread handles UI; background threads fetch data from APIs                       |

#### A Concrete Example — Music Player

Let's say you're building a simple music player in Java. Without multithreading:

```
Single-threaded music player:

  Play audio ───────────────────────────────────►
                    ↑
                    Can't update the UI, 
                    can't respond to "Next" button,
                    can't load the next song...
                    
                    App appears FROZEN! ❄️
```

With multithreading:

```
Multithreaded music player:

  Thread 1: Play audio      ────────────────────►
  Thread 2: Update UI       ────────────────────►
  Thread 3: Load next song  ────────────────────►
  Thread 4: Listen for input ───────────────────►

  Everything works smoothly! ✨
```

---

## 2. Difference between Process and Thread

### 🏠 Simple Analogy

Think of a **process** as a **house**.

- Each house has its own rooms, electricity, water, and storage.
- One house is mostly separate from another house.

Think of a **thread** as a **person working inside that house**.

- Multiple people can work inside the same house.
- They share the same rooms and resources.
- Because they share things, they must coordinate properly.

So:

```text
Process = Independent running program
Thread  = Small worker inside a process
```

---

### 📊 Detailed Comparison Table

| Point | Process | Thread | Simple Explanation |
|---|---|---|---|
| Basic meaning | A **process** is a program in execution state. | A **thread** is a subpart of a process and is the smallest unit of execution within a program. | When you open an application, the operating system creates a process. Inside that process, one or more threads do the actual work. |
| Weight | Process is **heavyweight**. | Thread is **lightweight**. | A process needs more separate resources from the operating system, while a thread uses many resources already available in its process. |
| Resource ownership | A process owns resources such as memory address space, code, open files/handles, and other operating-system resources. | A thread does not own a full separate resource set like a process; it shares the process resources. | Process is like a full house with its own resources. Thread is like a worker inside that house. |
| Address space | Each process has a **different/private address space**. | Threads of the same process share the **same address space**. | Processes are more isolated. Threads can access the same memory area inside their process. |
| Dependency | Processes are generally **independent** of each other. | Threads are generally **dependent** on each other because they exist inside the same process and share resources. | If one process crashes, another separate process may continue. But if a process ends, all its threads end with it. |
| Context switching | Process context switching usually takes **more time**. | Thread context switching usually takes **less time**. | Switching between processes is heavier because the operating system may need to switch more process-related information, such as memory context. Switching between threads of the same process is usually lighter. |
| What is context switching? | The CPU stops executing one process and starts executing another process. | The CPU stops executing one thread and starts executing another thread. | It happens so fast that users feel multiple tasks are running at the same time. |
| Communication | Inter-process communication usually takes **more time**. | Inter-thread communication usually takes **less time**. | Processes have separate memory, so they need mechanisms like pipes, sockets, files, shared memory, or message queues. Threads share memory, so communication can be faster, but it must be handled carefully. |
| Synchronization | Processes usually do not need synchronization if they do not share data. However, if processes share files, databases, shared memory, or other common resources, synchronization may be required. | Threads may require synchronization, especially when multiple threads access or modify the same shared resource. | The original point "process does not require synchronization" is mostly true for independent processes, but not always true when shared resources are involved. |
| Synchronization issue example | If two processes write to the same file at the same time, data can still become inconsistent unless access is controlled. | If multiple threads write to the same file or same variable at the same time, they may corrupt data or produce unexpected results. | Example: one thread is writing to a file while another thread closes the same file. To avoid this, we use synchronization. |
| Resource consumption | Resource consumption is **more** in a process. | Resource consumption is **less** in a thread. | Creating and managing a process needs more memory and operating-system resources. Threads are cheaper because they share many resources of the process. |
| Time of creation | Process requires **more time** for creation. | Thread requires **less time** for creation. | Starting a new process is like setting up a new house. Starting a new thread is like assigning another worker inside the same house. |
| Isolation and safety | Processes are more isolated and safer from each other. | Threads are less isolated because they share the same process memory. | A bug in one thread can affect other threads in the same process more easily. |
| Failure impact | Failure of one process usually does not directly corrupt another process. | Failure of one thread can affect the entire process and other threads inside it. | Since threads share memory, one faulty thread can damage shared data used by others. |
| Example | Opening Chrome, IntelliJ IDEA, VS Code, or a Java application creates processes. | A Java application may use separate threads for main logic, background tasks, file download, request handling, etc. | Process is the running application. Threads are workers inside that application. |

---

### 🔗 Multithreading Connection

In Java, when we talk about **multithreading**, we mean using multiple threads inside the same process.

```text
Java Application Process
│
├── Main Thread
├── File Download Thread
├── Music Playback Thread
└── User Interface Thread
```

All these threads belong to the same Java process, but they can perform different tasks.

---

### 🔒 Why Threads Need Synchronization

Threads share the same memory of a process. This is useful because sharing data becomes easy, but it also creates risk.

```text
Thread 1: Writing to file
Thread 2: Closing the same file
```

If both happen at the same time without control, the program may behave incorrectly.

That is why we use synchronization mechanisms such as:

- `synchronized` in Java
- Locks
- Thread-safe collections
- Atomic variables
- Semaphores

---

### 📝 Quick Summary

| Process | Thread |
|---|---|
| Independent running program | Small execution unit inside a process |
| Heavyweight | Lightweight |
| Has separate address space | Shares address space with other threads in same process |
| Communication is slower | Communication is faster |
| Creation takes more time | Creation takes less time |
| More resource consumption | Less resource consumption |
| More isolated | Less isolated |
| Usually safer from other processes | Needs careful synchronization when sharing data |

---

### 💡 One-Line Definition

> A **process** is a running program with its own resources, while a **thread** is a lightweight worker inside a process that shares the process resources.

---

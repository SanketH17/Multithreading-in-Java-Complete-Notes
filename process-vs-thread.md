# Process vs Thread

This note explains the difference between a **process** and a **thread** in a simple way.

## Simple Analogy

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

## Comparison Table

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
| Synchronization | Processes usually do not need synchronization if they do not share data. However, if processes share files, databases, shared memory, or other common resources, synchronization may be required. | Threads may require synchronization, especially when multiple threads access or modify the same shared resource. | The original point “process does not require synchronization” is mostly true for independent processes, but not always true when shared resources are involved. |
| Synchronization issue example | If two processes write to the same file at the same time, data can still become inconsistent unless access is controlled. | If multiple threads write to the same file or same variable at the same time, they may corrupt data or produce unexpected results. | Example: one thread is writing to a file while another thread closes the same file. To avoid this, we use synchronization. |
| Resource consumption | Resource consumption is **more** in a process. | Resource consumption is **less** in a thread. | Creating and managing a process needs more memory and operating-system resources. Threads are cheaper because they share many resources of the process. |
| Time of creation | Process requires **more time** for creation. | Thread requires **less time** for creation. | Starting a new process is like setting up a new house. Starting a new thread is like assigning another worker inside the same house. |
| Isolation and safety | Processes are more isolated and safer from each other. | Threads are less isolated because they share the same process memory. | A bug in one thread can affect other threads in the same process more easily. |
| Failure impact | Failure of one process usually does not directly corrupt another process. | Failure of one thread can affect the entire process and other threads inside it. | Since threads share memory, one faulty thread can damage shared data used by others. |
| Example | Opening Chrome, IntelliJ IDEA, VS Code, or a Java application creates processes. | A Java application may use separate threads for main logic, background tasks, file download, request handling, etc. | Process is the running application. Threads are workers inside that application. |

---

## Multithreading Connection

In Java, when we talk about **multithreading**, we mean using multiple threads inside the same process.

Example:

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

## Why Threads Need Synchronization

Threads share the same memory of a process. This is useful because sharing data becomes easy, but it also creates risk.

Example:

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

## Quick Summary

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

## One-Line Definition

A **process** is a running program with its own resources, while a **thread** is a lightweight worker inside a process that shares the process resources.

---
tags : [[Java/Java]], [[Java/Thread]], [[Java/Runnable]]

Multithreading is a process of executing multiple threads simultaneously.

#### What is Thread in java ?

A thread is a lightweight subprocess, the smallest unit of processing. It is a separate path of execution.
Threads are independent. If there occurs exception in one thread, it doesn't affect other threads. It uses a shared memory area.

![[attachments/Pasted image 20240504180632.png]]
As shown in the above figure, a thread is executed inside the process. There is context-switching between the threads. There can be multiple processes inside the OS, and one process can have multiple threads.

#### Advantages of Java Multithreading

1) It **doesn't block the user**/application because threads are independent and you can perform multiple operations at the same time.
2) You **can perform many operations together, so it saves time**.

## Multitasking

Multitasking is a process of executing multiple tasks simultaneously. We use multitasking to make efficient use the CPU. Multitasking can be achieved in two ways:

#### 1) Process-based Multitasking (Multiprocessing)

- Each process has an address in memory. In other words, each process allocates a separate memory area.
- A process is heavyweight.
- Cost of communication between the process is high.
- Switching from one process to another requires some time for saving and loading [registers](https://www.javatpoint.com/register-memory), memory maps, updating lists, etc.

#### 2) Thread-based Multitasking (Multithreading)

- Threads share the same address space.
- A thread is lightweight.
- Cost of communication between the thread is low.

## Java Thread class

Java provides **Thread class** to achieve thread programming. Thread class provides [constructors](https://www.javatpoint.com/java-constructor) and methods to create and perform operations on a thread. Thread class extends [Object class](https://www.javatpoint.com/object-class) and implements Runnable interface.

#### Life cycle of a Thread (Thread States)

In Java, a thread always exists in any one of the following states:

1. New
2. Active
3. Blocked / Waiting
4. Timed Waiting
5. Terminated

**<mark style="background: #FF5582A6;">New:</mark>** Whenever a new thread is created, it is always in the new state. For a thread in the new state, the code has not been run yet and thus has not begun its execution.

**<mark style="background: #FFB86CA6;">Active:</mark> When a thread invokes the start() method, it moves from the new state to the active state. The active state contains two states within it: one is **runnable**, and the other is **running**.

- **Runnable:** A thread, that is ready to run is then moved to the runnable state. It is the duty of the thread scheduler to provide the thread time to run, i.e., moving the thread the running state.  

- **Running:** When the thread gets the CPU, it moves from the runnable to the running state. Generally, the most common change in the state of a thread is from runnable to running and again back to runnable.

<mark style="background: #FFF3A3A6;">Blocked or Waiting:</mark> Whenever a thread is inactive for a span of time (not permanently) then, either the thread is in the blocked state or is in the waiting state.

- For example, a thread (let's say its name is A) may want to print some data from the printer. 
- However, at the same time, the other thread (let's say its name is B) is using the printer to print some data.
- Therefore, thread A has to wait for thread B to use the printer. 
- Thus, thread A is in the blocked state.

- When the main thread invokes the join() method then, it is in waiting state. 
- The main thread then waits for the child threads to complete their tasks. 
- When the child threads complete their job, a notification is sent to the main thread, which again moves the thread from waiting to the active state.

If there are a lot of threads in the waiting or blocked state, then it is the duty of the thread scheduler to determine which thread to choose and run.

**<mark style="background: #BBFABBA6;">Timed Waiting:</mark>** A real example of timed waiting is when we invoke the sleep() method on a specific thread. The sleep() method puts the thread in the timed wait state. After the time runs out, the thread wakes up and start its execution from when it has left earlier. If a thread has to wait forever, it leads to starvation. To avoid such scenario, a timed waiting state is given to thread

**<mark style="background: #ADCCFFA6;">Terminated:</mark>** A terminated thread means the thread is no more in the system. In other words, the thread is dead, and there is no way one can respawn (active after kill) the dead thread. A thread reaches the termination state because of the following reasons:

- When a thread has finished its job, then it exists or terminates normally.
- **Abnormal termination:** It occurs when some unusual events such as an unhandled exception or segmentation fault.


![[attachments/Pasted image 20240504183929.png]]

In Java, one can get the current state of a thread using the **Thread.getState()** method. The **java.lang.Thread.State** class of Java provides the [[Enumeration]] constants to represent the state of a thread.

## How to create a thread in Java ?

There are two ways to create a thread:

1. By extending [[Java/Thread]] class
2. By implementing [[Java/Runnable]] interface.

If you are not extending the Thread class, your class object would not be treated as a thread object. So you need to explicitly create the Thread class object. 

Or pass the object of your class that implements Runnable to a thread constructor so that your class run() method may execute.

## How to perform single task by multiple threads in Java?

If you have to perform a single task by many threads, have only one run() method. Use notify and [[Java/Synchronization]]

## How to perform multiple tasks by multiple threads (multitasking in multithreading)?

If you have to perform multiple tasks by multiple threads, have multiple run() methods




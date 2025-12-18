---
{"publish":true,"title":"Concurrency & Multi-Threading","cssclasses":""}
---

## Definitions

**Thread**: Lightweight process executing code independently. Multiple threads share same process memory. Enables concurrent execution.

**Multithreading**: Multiple threads executing simultaneously (or appearing simultaneous in single-core). Increases responsiveness and throughput.

**Thread Lifecycle**: NEW → RUNNABLE → BLOCKED/WAITING/TIMED_WAITING → TERMINATED.

**synchronized Keyword**: Method or block restricting access to single thread. Enforces mutual exclusion and happens-before relationship.

**volatile Keyword**: Variables ensuring visibility of writes across threads. Prevents caching; reads/writes always from main memory.

**Race Condition**: Multiple threads accessing shared data simultaneously without synchronization, causing unpredictable results.

---

## QnA

**How do you create a thread in Java?**

Extend Thread class or implement Runnable interface (preferred for multiple inheritance). Call `start()` method (not `run()`) to create new thread and invoke run() asynchronously. For example, `new Thread(new MyTask()).start();` creates and starts a new thread. Never call run() directly—it executes in current thread.

**What's the difference between Thread.start() and Thread.run()?**

`start()` creates a new thread and calls run() asynchronously in that thread. `run()` executes in the current thread, blocking until complete. Always use start() for concurrent execution; run() directly defeats threading purposes. This is a common mistake causing single-threaded behavior.

**What does the volatile keyword do?**

Volatile ensures a variable's visibility across threads—writes become immediately visible to other threads. It prevents JVM from caching the variable value; reads and writes always access main memory. Use volatile for flags or simple values. Note: volatile doesn't guarantee atomicity, only visibility.

**What's the difference between synchronized and volatile?**

Synchronized provides both mutual exclusion (only one thread at a time) and visibility. Volatile provides only visibility, not atomicity. Use synchronized for shared data being modified; use volatile for flags or simple communication. Synchronized is more expensive; use judiciously.

**What causes deadlock?**

Thread A waits for lock held by B; B waits for lock held by A. Circular wait prevents progress. Deadlocks are hard to detect and reproduce. Prevention strategies: consistent lock ordering (always acquire locks in same order), avoid nested locks, use timeouts (tryLock), or use lock-free algorithms.

**How do you prevent deadlock?**

Strategies include: maintaining consistent lock ordering across threads, avoiding nested/multiple lock acquisitions, using timeouts with tryLock(), using concurrent data structures (ConcurrentHashMap), or lock-free algorithms. Design is critical—prevent deadlock scenarios rather than trying to recover from them.

---

## Code Examples

**Example - Creating & Starting Threads:**
```java
// Approach 1: Extend Thread
class MyThread extends Thread {
    public void run() {
        System.out.println("Thread: " + Thread.currentThread().getName());
    }
}
MyThread thread = new MyThread();
thread.start();  // Correct

// Approach 2: Implement Runnable (preferred)
class MyTask implements Runnable {
    public void run() {
        System.out.println("Task running");
    }
}
new Thread(new MyTask()).start();

// Approach 3: Lambda (Java 8+)
new Thread(() -> {
    System.out.println("Lambda thread");
}).start();
```

**Example - synchronized Method:**
```java
public class Counter {
    private int count = 0;
    
    // synchronized ensures only one thread at a time
    public synchronized void increment() {
        count++;  // Safe from race conditions
    }
    
    public synchronized int getCount() {
        return count;
    }
}

// Concurrent calls
Counter counter = new Counter();
for (int i = 0; i < 100; i++) {
    new Thread(() -> {
        for (int j = 0; j < 1000; j++) {
            counter.increment();
        }
    }).start();
}
// Guaranteed: count = 100,000 (thread-safe)
```

**Example - synchronized Block:**
```java
public class Account {
    private int balance = 1000;
    
    public void transfer(Account to, int amount) {
        synchronized (this) {
            if (balance >= amount) {
                balance -= amount;
            }
        }
        synchronized (to) {
            to.balance += amount;
        }
    }
}
```

**Example - volatile Keyword:**
```java
public class TaskRunner {
    private volatile boolean running = true;  // Visibility
    
    public void stop() {
        running = false;  // All threads see immediately
    }
    
    public void run() {
        while (running) {
            // Work continues
        }
    }
}
```

---

## Key Points

- **Thread States**: NEW, RUNNABLE, BLOCKED, WAITING, TIMED_WAITING, TERMINATED. State transitions crucial for understanding concurrency.
- **happens-before**: Synchronized ensures all changes visible before lock release; thread-safe memory visibility.
- **Intrinsic Locks**: Every object has built-in lock (monitor). Synchronized acquires object's lock automatically.
- **Thread Scheduling**: JVM/OS decides execution order; non-deterministic. Assume worst-case interleaving always.
- **Synchronization Overhead**: Locking has performance cost; use only when necessary. Consider lock-free alternatives for high contention.

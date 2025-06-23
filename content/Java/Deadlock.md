tags: [[Thread]]. [[Multithreading]], [[Synchronization]]

### **What is a deadlock?**

- A **deadlock** occurs when two or more threads are **blocked forever**, each waiting for the other to release a lock.
- Since, both threads are waiting for each other to release the lock, the condition is called deadlock.

### **What are the conditions required for a deadlock to occur?**

Four conditions must be true **simultaneously** for a deadlock to happen:
- **Mutual exclusion**: Resources cannot be shared.
- **Hold and wait**: A thread holds one lock and waits for another.
- **No preemption**: Locks cannot be forcibly removed from threads.
- **Circular wait**: A circular chain of threads exists, each waiting on another.

### **Can you give a simple example of deadlock in Java?**

```run-java
public class TestDeadlockExample1 {  
  public static void main(String[] args) {  
    final String resource1 = "resource 1";  
    final String resource2 = "resource 2";  
    
    // t1 tries to lock resource1 then resource2  
    Thread t1 = new Thread() {  
      public void run() {  
          synchronized (resource1) {  
           System.out.println(Thread.currentThread() + " lock on " + resource1);  
  
           try { Thread.sleep(100);} catch (Exception e) {}  
  
           synchronized (resource2) {  
            System.out.println(Thread.currentThread() + " lock on " + resource2);  
           }  
         }  
      }  
    };  
  
    // t2 tries to lock resource2 then resource1  
    Thread t2 = new Thread() {  
      public void run() {  
        synchronized (resource2) {  
          System.out.println(Thread.currentThread() + " lock on " + resource2);  
  
          try { Thread.sleep(100);} catch (Exception e) {}  
  
          synchronized (resource1) {  
            System.out.println(Thread.currentThread() + " lock on " + resource1);  
          }  
        }  
      }  
    };  
  
      
    t1.start();  
    t2.start();  
  }  
}       
```

### **How to avoid deadlock?**

Deadlocks cannot be completely resolved. But we can avoid them by following basic rules mentioned below:

1. **Avoid Nested Locks**: We must avoid giving locks to multiple threads, this is the main reason for a deadlock condition. It normally happens when you give locks to multiple threads.
2. **Avoid Unnecessary Locks**: The locks should be given to the important threads. Giving locks to the unnecessary threads that cause the deadlock condition.
3. **Using Thread Join**: A deadlock usually happens when one thread is waiting for the other to finish. In this case, we can use **join** with a maximum time that a thread will take.

### **What is ReentrantLock and how does it help with deadlocks?**

`ReentrantLock` is a class from `java.util.concurrent.locks` that allows:
- **Explicit locking/unlocking**
- **tryLock(timeout)** which avoids waiting forever and can break potential deadlocks

```java
if (lock1.tryLock(1, TimeUnit.SECONDS)) {
    try {
        if (lock2.tryLock(1, TimeUnit.SECONDS)) {
            try {
                // critical section
            } finally {
                lock2.unlock();
            }
        }
    } finally {
        lock1.unlock();
    }
}
```

### **What is the difference between deadlock and livelock?**

| Concept | Deadlock                                  | Livelock                                 |
| ------- | ----------------------------------------- | ---------------------------------------- |
| Meaning | Threads stop progressing, blocked forever | Threads keep changing state, no progress |
| Example | Waiting on each other’s lock              | Constantly retrying and yielding         |

### **What is the difference between deadlock and starvation?**

| Concept | Deadlock                                 | Starvation                              |
| ------- | ---------------------------------------- | --------------------------------------- |
| Cause   | Mutual blocking of threads               | Thread is unable to gain regular access |
| State   | Threads are stuck, waiting on each other | One thread never gets CPU or lock time  |
| Example | Circular lock wait                       | Low-priority thread never executes      |

tags: [[Thread]]. [[Multithreading]], [[Synchronization]]

Deadlock is a part of multithreading. Deadlock can occur in a situation when a thread is waiting for an object lock, that is acquired by another thread and second thread is waiting for an object lock that is acquired by first thread. 

Since, both threads are waiting for each other to release the lock, the condition is called deadlock.

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

#### How to avoid deadlock?

Deadlocks cannot be completely resolved. But we can avoid them by following basic rules mentioned below:

1. **Avoid Nested Locks**: We must avoid giving locks to multiple threads, this is the main reason for a deadlock condition. It normally happens when you give locks to multiple threads.
2. **Avoid Unnecessary Locks**: The locks should be given to the important threads. Giving locks to the unnecessary threads that cause the deadlock condition.
3. **Using Thread Join**: A deadlock usually happens when one thread is waiting for the other to finish. In this case, we can use **join** with a maximum time that a thread will take.
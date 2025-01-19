tags :[[Multithreading]]

## Thread class:

Thread class provide constructors and methods to create and perform operations on a thread.Thread class extends Object class and implements Runnable interface.

#### Commonly used Constructors of Thread class:

- Thread()
- Thread(String name)
- Thread(Runnable r)
- Thread(Runnable r,String name)

#### Commonly used methods of Thread class:

1. **public void run():** is used to perform action for a thread.

3. **public void start():** starts the execution of the thread.JVM calls the run() method on the thread.

4. **public void sleep(long miliseconds):** Causes the currently executing thread to sleep (temporarily cease execution) for the specified number of milliseconds.

5. **public void join():** waits for a thread to die.

6. **public void join(long miliseconds):** waits for a thread to die for the specified miliseconds.

7. **public int getPriority():** returns the priority of the thread.

8. **public int setPriority(int priority):** changes the priority of the thread.

9. **public String getName():** returns the name of the thread.

10. **public void setName(String name):** changes the name of the thread.

11. **public Thread currentThread():** returns the reference of currently executing thread.

12. **public int getId():** returns the id of the thread.

13. **public Thread.State getState():** returns the state of the thread.

14. **public boolean isAlive():** tests if the thread is alive.

15. **public void yield():** causes the currently executing thread object to temporarily pause and allow other threads to execute.

16. **public void suspend():** is used to suspend the thread(depricated).

17. **public void resume():** is used to resume the suspended thread(depricated).

18. **public void stop():** is used to stop the thread(depricated).

19. **public boolean isDaemon():** tests if the thread is a daemon thread.

20. **public void setDaemon(boolean b):** marks the thread as daemon or user thread.

21. **public void interrupt():** interrupts the thread.

22. **public boolean isInterrupted():** tests if the thread has been interrupted.

23. **public static boolean interrupted():** tests if the current thread has been interrupted.


```run-java
class Multi extends Thread{  
	public void run(){  
	System.out.println("thread is running...");  
	}  

	public static void main(String args[]){  
	Multi t1=new Multi();  
	t1.start();  
	}  
}
```

#### Thread.sleep()

he method sleep() is being used to halt the working of a thread for a given amount of time. The time up to which the thread remains in the sleeping state is known as the sleeping time of the thread. After the sleeping time is over, the thread starts its execution from where it has left.

Whenever another thread does interruption while the current thread is already in the sleep mode, then the InterruptedException is thrown.

IllegalArguementException is thrown if time (-1ms) is a negative value.

#### Can we start a thread twice

No. After starting a thread, it can never be started again. If you does so, an _IllegalThreadStateException_ is thrown. In such case, thread will run once but for second time, it will throw exception.

#### What if we call Java run() method directly instead start() method?

- Each thread <mark style="background: #FFB86CA6;">starts</mark> in a separate call stack.
- Invoking the run() method from the main thread, the run() method goes onto the current call stack rather than at the beginning of a new call stack.

![[Pasted image 20240511112620.png]]

#### Thread.join()

When the join() method is invoked, the current thread stops its execution and the thread goes into the wait state. The current thread remains in the wait state until the thread on which the join() method is invoked has achieved its dead state.


```run-java
class TestCallRun2 extends Thread{  
 public void run(){  
  for(int i=1;i<5;i++){  
    try{Thread.sleep(500);}catch(InterruptedException e){System.out.println(e);}  
    System.out.println(i);  
  }  
 }  
 public static void main(String args[]){  
  TestCallRun2 t1=new TestCallRun2();  
  TestCallRun2 t2=new TestCallRun2(); 
  TestCallRun2 t3=new TestCallRun2();

  System.out.println("t1, t2 not running in seperate threads"); 
  t1.run();  
  t2.run();

  System.out.println("t3 runs in seperate thread");
  t3.start();
  try  
  {
   System.out.println("makes current thread (main) halt until t3 finishes");
   t3.join();  
  }   
  catch(Exception e)  
  {  
   System.out.println("The exception has been caught " + e);  
  }

  System.out.println("t1, t2 runs in sperate thread");
  t1.start();
  t2.start();

 }  
}  
```


#### Thread.priority()

3 constants defined in Thread class:

1. public static int MIN_PRIORITY - 1
2. public static int NORM_PRIORITY - 5
3. public static int MAX_PRIORITY - 10

## Daemon Thread

- It provides services to other threads for background supporting tasks. It has no role in life than to serve user threads.
- Its life depends on user threads.
- It is a low priority thread.

If you want to make a user thread as Daemon, it must be done before starting the thread otherwise it will throw IllegalThreadStateException.

## Interrupting a Thread:

- If any thread is in sleeping or waiting state (i.e. sleep() or wait() is invoked), calling the interrupt() method on the thread, breaks out the sleeping or waiting state throwing InterruptedException. 
- If the thread is not in the sleeping or waiting state, calling the interrupt() method performs normal behaviour and doesn't interrupt the thread but sets the interrupt flag to true.
- **public void interrupt()** - will Interrupt the thread.
- **public static boolean interrupted()** - returns the interrupted flag afterthat it sets the flag to false if it is true.
- **public boolean isInterrupted()** - returns the interrupted flag either true or false.

## Java Monitors:

- A **monitor** in Java is like a **guardian** for shared resources.
- It ensures that only one thread can access a shared resource at a time.
- Monitors provide mutual exclusion and coordination for concurrent programs.

#### Reentrant Monitors:

- **Reentrant** means a thread can **re-enter** the same monitor it already holds.
- In other words, if a thread is inside a synchronized method (holding the monitor), it can call another synchronized method without blocking itself.
- Consider a scenario where a synchronized method calls another synchronized method.
- Without reentrant monitors, the inner method would block itself because it already holds the monitor.
- This reentrant behavior prevents deadlocks and simplifies code.

 **Example**:
- Imagine a class with synchronized methods `m()` and `n()`.
- If `m()` calls `n()`, the same thread can safely re-enter the monitor.
- This ensures that the thread doesn’t get stuck waiting for itself.

**Advantages**:
- **Deadlock Prevention**: Reentrant monitors eliminate the risk of a single thread deadlocking on a monitor it already holds.
- **Simplicity**: They simplify synchronization by allowing nested method calls within the same monitor.
tags : [[Thread]]

**Thread pool** is a group of worker threads that can be reused many times.

In the case of a thread pool, a group of fixed-size threads is created. A thread from the thread pool is pulled out and assigned a job by the service provider. After completion of the job, the thread is contained in the thread pool again.

en one wants to execute 50 tasks but is not willing to create 50 threads. In such a case, one can create a pool of 10 threads. Thus, 10 out of 50 tasks are assigned, and the rest are put in the queue. Whenever any thread out of 10 threads becomes idle, it picks up the 11th task and so on.
## Thread Pool Methods

**newFixedThreadPool(int s):** The method creates a thread pool of the fixed size s.

**newCachedThreadPool():** The method uses previously created threads in the pool. If no thread is free in the pool, it creates a new thread.

**newSingleThreadExecutor():** The method creates a new thread.

Thread Pools in Java are managed by [[ExecutorService]].  ExecutorService helps in executing tasks in asynchronous mode.

## Risks involved in Thread Pools

**Deadlock:** - Deadlock occurs when multiple threads are blocked, waiting for each other to release resources.

**Thread Leakage:** Leakage of threads occurs when a thread is being removed from the pool to execute a task but is not returning to it after the completion of the task. For example, when a thread throws the exception and the pool class is not able to catch this exception, then the thread exits and reduces the thread pool size by 1. If the same thing repeats a number of times, then there are fair chances that the pool will become empty, and hence, there are no threads available in the pool for executing other requests.

**Resource thrashing:** occurs when too many threads fight for limited resources, causing unnecessary context switching and slowing down the system. It’s like having too many employees in a small office, constantly changing tasks without getting much done!

## Tuning the Thread Pool

The accurate size of a thread pool is decided by the number of available processors and the type of tasks the threads have to execute. 
If a system has the P processors that have only got the computation type processes, then the maximum size of the thread pool of P or P + 1 achieves the maximum efficiency. 
However, the tasks may have to wait for I/O, and in such a scenario, one has to take into consideration the ratio of the waiting time (W) and the service time (S) for the request; resulting in the maximum size of the pool P * (1 + W / S) for the maximum efficiency.

## Thread pools and resource management:

1. **Avoid Concurrently Waiting Tasks**:
    - **Don’t queue tasks** that are waiting for results from other tasks.
    - This can lead to a **deadlock situation**, where threads block each other indefinitely.
    
1. **Long-Lived Threads**:
    - Be cautious when using threads for long-lived operations.
    - Threads waiting forever can cause **resource leaks**.

3. **Explicitly End Thread Pools**:
    - Always **shut down** the thread pool explicitly.
    - Otherwise, the program may keep running indefinitely.
    - Use the `shutdown()` method to terminate the executor.

4. **Task Understanding and Tuning**:
    - Understand your tasks to **tune the thread pool** effectively.
    - If tasks have different characteristics, consider separate pools for different task types.

5. **Memory Management**:
    - Control the **maximum threads** running in the JVM.
    - Prevent running out of memory.
    - The thread pool won’t create new threads beyond the limit.

6. **Reuse Threads**:
    - A thread pool can **reuse** threads that have finished execution.
    - Saves time and resources compared to creating new threads.
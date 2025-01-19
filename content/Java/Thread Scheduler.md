tags: [[Thread]], [[Multithreading]]

A component of Java that decides which thread to run or execute and which thread to wait is called a **thread scheduler**

 if there is more than one thread in the runnable state, it is up to the thread scheduler to pick one of the threads and ignore the other ones. There are some criteria that decide which thread will execute first. There are two factors for scheduling a thread i.e. **Priority** and **Time of arrival**.

**Priority:** Priority of each thread lies between 1 to 10. If a thread has a higher priority, it means that thread has got a better chance of getting picked up by the thread scheduler.

**Time of Arrival:** Suppose two threads of the same priority enter the runnable state, then priority cannot be the factor to pick a thread from these two threads. In such a case, **arrival time** of thread is considered by the thread scheduler.

## Thread Scheduler Algorithms

#### First Come First Serve Scheduling:

In this scheduling algorithm, the scheduler picks the threads thar arrive first in the runnable queue. Observe the following table:

|Threads|Time of Arrival|
|---|---|
|t1|0|
|t2|1|
|t3|2|
|t4|3|
t1 -> t2 -> t3 -> t4

#### Time-slicing scheduling:
<span class="smallimg"><span class="leftimg"> ![[Pasted image 20240511104248.png]] </span></span>

Usually, the First Come First Serve algorithm is non-preemptive, which is bad as it may lead to infinite blocking (also known as starvation). 

To avoid that, some time-slices are provided to the threads so that after some time, the running thread has to give up the CPU. 

Thus, the other waiting threads also get time to run their job.

#### Preemptive-Priority Scheduling:

Suppose there are multiple threads available in the runnable state. The thread scheduler picks that thread that has the highest priority. Since the algorithm is also preemptive, therefore, time slices are also provided to the threads to avoid starvation. Thus, after some time, even if the highest priority thread has not completed its job, it has to release the CPU because of preemption.

![[Pasted image 20240511104443.png]]


## Working of the Java Thread Scheduler

Suppose, there are five threads that have different arrival times and different priorities. Now, it is the responsibility of the thread scheduler to decide which thread will get the CPU first.

The thread scheduler selects the thread that has the highest priority, and the thread begins the execution of the job. If a thread is already in runnable state and another thread (that has higher priority) reaches in the runnable state, then the current thread is pre-empted from the processor, and the arrived thread with higher priority gets the CPU time.

When two threads having the same priorities and arrival time, the scheduling will be decided on the basis of FCFS algorithm. Thus, the thread that arrives first gets the opportunity to execute first.


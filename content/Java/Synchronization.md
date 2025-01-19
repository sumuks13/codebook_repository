tags: [[Multithreading]]

Synchronization is the capability to control/restrict the access of multiple threads to any shared resource.

The synchronization is mainly used to

1. To prevent thread interference.
2. To prevent data consistency problem.

There are two types of synchronization

1. Process Synchronization
2. Thread Synchronization

#### Thread Synchronization

There are two types of thread synchronization mutual exclusive and inter-thread communication.

1. **Mutual Exclusive**: helps keep threads from interfering with one another while sharing data. It can be achieved by using the following three ways:
	1. **By Using Synchronized Method** : Synchronized method is used to lock an object for any shared resource inside the method.
	2. **By Using Synchronized Block** : Suppose we have 50 lines of code in our method, but we want to synchronize only 5 lines, in such cases, we can use synchronized block.
	3. **By Using Static Synchronization** : If you make any static method as synchronized, the lock will be on the class not on object.

1. **Cooperation** (Inter-thread communication in java) : it is a mechanism in which a thread is paused running in its critical section and another thread is allowed to enter (or lock) in the same critical section to be executed. It is implemented by following methods of **Object class**:
	1. wait()
	2. notify()
	3. notifyAll()

- The system performance may degrade because of the slower working of synchronized keyword.
- Java synchronized block is more efficient than Java synchronized method.

#### Concept of Lock in Java

Every object has a lock associated with it. By convention, a thread that needs consistent access to an object's fields has to acquire the object's lock before accessing them, and then release the lock when it's done with them.

From Java 5 the package java.util.concurrent.locks contains several lock implementations.

```run-java
//Program of synchronized method by using annonymous class  
public class TestSynchronization3{  
public static void main(String args[]){  
final Table obj = new Table();//only one object
final Table obj2 = new Table(); //for static method
  
Thread t1=new Thread(){  
public void run(){  
obj.printTable(5);  
}  
};  
Thread t2=new Thread(){  
public void run(){  
obj.printTable(100);  
}  
};  
/**Thread t3 = new Thread(){
public void run(){
obj2.printTable(10);
}
};**/
  
t1.start();  
t2.start();
//t3.start();
}  
}

class Table{  
 /**synchronized**/ void printTable(int n){//synchronized method  
   for(int i=1;i<=5;i++){  
     System.out.println(n*i);  
     try{  
      Thread.sleep(400);  
     }catch(Exception e){System.out.println(e);}  
   }  
  
 }  
}  
```

Synchronized Static 

```run-java
public class TestSynchronization5 {  
public static void main(String[] args) {  
      
    Thread t1=new Thread(){  
        public void run(){  
            Table.printTable(1);  
        }  
    };  
      
    Thread t2=new Thread(){  
        public void run(){  
            Table.printTable(10);  
        }  
    };  
      
    Thread t3=new Thread(){  
        public void run(){  
            Table.printTable(100);  
        }  
    };  

    t1.start();  
    t2.start();  
    t3.start(); 
}  
}  

class Table{  
  
 synchronized static  void printTable(int n){  
   for(int i=1;i<=10;i++){  
     System.out.println(n*i);  
     try{  
       Thread.sleep(400);  
     }catch(Exception e){}  
   }  
 }  
}  
```

#### Synchronized block on a class lock:

The block synchronizes on the lock of the object denoted by the reference .class. 
A static synchronized method printTable(int n) in class Table is equivalent to the following declaration:

```java
static void printTable(int n) {  
    synchronized (Table.class) {  // Synchronized block on class A  
        // ...  
    }  
}  
```

## Inter-Thread Communication

#### 1) wait() method

The wait() method causes current thread to release the lock and wait until either another thread invokes the notify() method or the notifyAll() method for this object, or a specified amount of time has elapsed.

The current thread must own this object's monitor, so it must be called from the synchronized method only otherwise it will throw exception.

#### 2) notify() method

The notify() method wakes up a single thread that is waiting on this object's monitor. If any threads are waiting on this object, one of them is chosen to be awakened. The choice is arbitrary and occurs at the discretion of the implementation.

#### 3) notifyAll() method

Wakes up all threads that are waiting on this object's monitor.

#### Understanding the process of inter-thread communication

![inter thread communication in java](https://static.javatpoint.com/core/images/inter-thread-communication-in-java.png)

The point to point explanation of the above diagram is as follows:

1. Threads enter to acquire lock.
2. Lock is acquired by on thread.
3. Now thread goes to waiting state if you call wait() method on the object. Otherwise it releases the lock and exits.
4. If you call notify() or notifyAll() method, thread moves to the notified state (runnable state).
5. Now thread is available to acquire lock.
6. After completion of the task, thread releases the lock and exits the monitor state of the object.


## 1. Why wait(), notify() and notifyAll() methods are defined in Object class not Thread class?

It is because they are related to **lock** and object has a lock.

## 2. Difference between wait and sleep?

Let's see the important differences between wait and sleep methods.

| wait()                                                   | sleep()                                                 |
| -------------------------------------------------------- | ------------------------------------------------------- |
| The wait() method releases the lock.                     | The sleep() method doesn't release the lock.            |
| It is a method of Object class                           | It is a method of Thread class                          |
| It is the non-static method                              | It is the static method                                 |
| It should be notified by notify() or notifyAll() methods | After the specified amount of time, sleep is completed. |

```run-java
class Test{    
	public static void main(String args[]){    
		final Customer c=new Customer();    
		new Thread(){    
			public void run(){c.withdraw(15000);}    
		}.start();    
		
		new Thread(){    
			public void run(){c.deposit(10000);}    
		}.start();    
	}
}  

class Customer{    
	int amount=10000;    
    
	synchronized void withdraw(int amount){    
		System.out.println("going to withdraw...");    
    
		if(this.amount<amount){    
			System.out.println("Less balance; waiting for deposit...");    
			try{
				wait();
			}catch(Exception e){}    
		}    
		this.amount-=amount;    
		System.out.println("withdraw completed...");    
	}    
    
	synchronized void deposit(int amount){    
		System.out.println("going to deposit...");    
		this.amount+=amount;    
		System.out.println("deposit completed... ");    
		notify();    
	}    
}    
```

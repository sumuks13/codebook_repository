tags: [[Java]], [[Memory Management]]

Garbage Collector is Java's Automatic Memory management system that allocates and de-allocates Objects.

When a program executes in Java, it uses memory in different ways. The heap is a part of memory where objects live. It's the only part of memory that involved in the garbage collection process. All the garbage collection makes sure that the heap has as much free space as possible. The function of the garbage collector is to find and delete the objects that cannot be reached.

JVM controls the garbage collector. JVM decides when to perform the garbage collection. We can also request to the JVM to run the garbage collector. But there is no guarantee under any conditions that the JVM will comply. JVM runs the garbage collector if it senses that memory is running low.
### Object Allocation

When an object allocates, the JRockit JVM checks the size of the object. It distinguishes between small and large objects. The small and large size depends on the JVM version, heap size, garbage collection strategy, and platform used. The size of an object is usually between 2 to 128 KB.

The small objects are stored in Thread Local Area (TLA) which is a free chunk of the heap. TLA does not synchronize with other threads. When TLA becomes full, it requests for new TLA.
On the other hand, large objects that do not fit inside the TLA directly allocated into the heap.

### When is an Object eligible for GC ?

We can say that an object is eligible for garbage collection when no live thread can access it. The garbage collector considers that object as eligible for deletion. If a program has a reference variable that refers to an object, that reference variable available to live thread, this object is called **reachable**.

### Types of Garbage Collection

There are five types of garbage collection are as follows:

- **Serial GC:** It uses the mark and sweeps approach for young and old generations, which is minor and major GC.
- **Parallel GC:** It is similar to serial GC except that, it spawns N (the number of CPU cores in the system) threads for young generation garbage collection.
- **Parallel Old GC:** It is similar to parallel GC, except that it uses multiple threads for both generations.
- **Concurrent Mark Sweep (CMS) Collector:** It does the garbage collection for the old generation. It is also known as Concurrent Low Pause Collector.
- **G1 Garbage Collector:** It introduced in Java 7. Its objective is to replace the CMS collector. It is a parallel, concurrent, and CMS collector. There is no young and old generation space. It divides the heap into several equal sized heaps. It first collects the regions with lesser live data.

### What are the common Garbage Collection tuning parameters in Java?

A: The following are some common Garbage Collection tuning parameters in Java:

-Xmx: Sets the maximum heap size
-Xms: Sets the initial heap size
-XX:MaxGCPauseMillis: Sets the maximum pause time for Garbage Collection
-XX:+UseConcMarkSweepGC: Uses Concurrent Mark Sweep Garbage Collection policy
-XX:+UseG1GC: Uses G1 Garbage Collection policy
-XX:NewRatio: Sets the ratio of the young generation to the old generation
-XX:SurvivorRatio: Sets the ratio of survivor space to Eden space

How can an object be unreferenced?

There are many ways:

By nulling the reference
By assigning a reference to another
By anonymous object etc

1) By nulling a reference:

Employee e=new Employee();  
e=null;  

2) By assigning a reference to another:

Employee e1=new Employee();  
Employee e2=new Employee();  
e1=e2;//now the first object referred by e1 is available for garbage collection 

3) By anonymous object:
new Employee();  

finalize() method:

The finalize() method is invoked each time before the object is garbage collected. This method can be used to perform cleanup processing.

gc() method

The gc() method is used to invoke the garbage collector to perform cleanup processing. The gc() is found in System and Runtime classes.

```java
public class TestGarbage1{  
  public void finalize(
   {System.out.println("object is garbage collected");}  
 public static void main(String args[]){  
   TestGarbage1 s1=new TestGarbage1();    TestGarbage1 s2=new TestGarbage1();  
   s1=null;  
  s2=null;  
   System.gc();  
  }  
 }
```

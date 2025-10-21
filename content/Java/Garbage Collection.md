tags: [[Java/Java]], [[Java/Memory Management]]

Garbage Collector is Java's Automatic Memory management system that allocates and de-allocates Objects.

- When a program executes in Java, it uses memory in different ways. 
- The heap is a part of memory where objects live. It's the only part of memory that involved in the garbage collection process. 
- All the garbage collection makes sure that the heap has as much free space as possible. 
- The function of the garbage collector is to find and delete the objects that cannot be reached.

- JVM controls the garbage collector. 
- JVM decides when to perform the garbage collection. 
- We can also request to the JVM to run the garbage collector. But there is no guarantee under any conditions that the JVM will comply. 
- JVM runs the garbage collector if it senses that memory is running low.

- When an object allocates, the JVM checks the size of the object. 
- It distinguishes between small and large objects. The small and large size depends on the JVM version, heap size, garbage collection strategy, and platform used. 
- The size of an object is usually between 2 to 128 KB.

- The small objects are stored in Thread Local Area (TLA) which is a free chunk of the heap. 
- TLA does not synchronize with other threads. 
- When TLA becomes full, it requests for new TLA.
- On the other hand, large objects that do not fit inside the TLA directly allocated into the heap.

### **What is garbage collection in Java?**

Garbage collection (GC) is the process by which the Java Virtual Machine (JVM) **automatically frees up memory** by **removing objects that are no longer reachable** or used in the program.

### **Why is garbage collection important?**

- Prevents **memory leaks**
- Frees developers from manual memory management
- Improves **application performance** and **stability**

### **What is eligible for garbage collection?**

An object is eligible for GC when **no active part of the code can access it**.  
Common scenarios:

- Object has no references
- Reference is explicitly set to `null`
- The reference variable goes out of scope

### **How can an object be unreferenced?**

1) By nulling a reference:

```java
Employee e=new Employee();  
e=null;
```

2) By assigning a reference to another:

```java
Employee e1=new Employee();  
Employee e2=new Employee();  
e1=e2; //now the first object referred by e1 is available for garbage collection 
```

3) By anonymous object:

```java
new Employee();  
```

### **Does Java provide a way to force garbage collection?**

You can **request** GC using the below method. But it’s **only a suggestion**, not a guarantee. JVM decides **when** to run it.

```java
System.gc();
```

### **What is the garbage collector in Java?**

It is a part of JVM responsible for **automatically reclaiming memory** used by unreachable objects.  
It works in the **background** and uses different algorithms depending on the JVM implementation.

### **Can you manually delete an object in Java?**

No. Java does **not support manual memory deallocation**. All memory cleanup is handled **automatically by the JVM**.

### **What are some common garbage collection algorithms used in Java?**

- **Mark and Sweep**
- **Generational Garbage Collection**
- **G1 (Garbage First) Collector**
- **ZGC** and **Shenandoah** (low-latency collectors)

### **What is the Mark and Sweep algorithm?**

1. **Mark**: Find all objects that are still reachable
2. **Sweep**: Delete all objects that are not marked

### **What are the parts of Java heap memory related to GC?**

| Area            | Description                                      |
| --------------- | ------------------------------------------------ |
| Eden Space      | Where new objects are first allocated (YoungGen) |
| Survivor Spaces | Hold objects that survived GC in Eden (YoungGen) |
| Old Generation  | Holds long-lived objects                         |
| Metaspace       | Stores class metadata (replaces PermGen)         |

### **What is Generational Garbage Collection?**

Java divides the heap into **generations** to optimize GC. In generational GC, young generation is collected more frequently than the old generation.

### **What is the G1 Garbage Collector?**

- G1 (Garbage First) is a **server-style GC**
- Breaks the heap into **regions**, collects in parallel
- Designed for **predictable pause times** and **large heaps**

### **What is finalization in Java?**

The `finalize()` method is called by GC **before an object is removed** from memory. `finalize()` is deprecated since Java 9 and removed in Java 18+

```java
@Override
protected void finalize() throws Throwable {
    System.out.println("Object is being collected");
}
```

### **What is the difference between finalize() and gc() methods?**

- The finalize() method is invoked each time before the object is garbage collected. This method can be used to perform cleanup processing.
- The gc() method is used to invoke the garbage collector to perform cleanup processing. The gc() is found in System and Runtime classes.

```run-java
public class TestGarbage1{
	
	@Override
	protected void finalize() throws Throwable{
		System.out.println("object is garbage collected");
	}
	
	public static void main(String args[]){
		TestGarbage1 s1 = new TestGarbage1();
		TestGarbage1 s2 = new TestGarbage1();

		s1=null;
		s2=null;
		System.gc();
		}
 }
```

### **How do soft, weak, and phantom references affect GC?**

|Type|Collected when?|
|---|---|
|**SoftReference**|When memory is low|
|**WeakReference**|On next GC cycle, even if memory is fine|
|**PhantomReference**|After object is finalized|

These are in the `java.lang.ref` package and are used for caching or memory-sensitive applications.

### **What is memory leak in Java?**

When objects are **not garbage collected** because they are still referenced, even though they're no longer needed.

Example: Adding objects to a `static List` and never removing them.

### **Does garbage collection guarantee no memory leaks?**

No. GC cleans only **unreachable objects**. If objects are still referenced (even unintentionally), they will not be collected — resulting in **logical memory leaks**.

### **Can static variables be garbage collected?**

Only when the **entire class loader is eligible for GC** they become eligible.  Otherwise, static fields are **root references** and remain in memory.

### **What are GC roots in Java?**

GC starts from **root references** (GC roots), which include:
- Static fields
- Local variables on thread stacks
- Active threads
- JNI references

Any object not reachable from a GC root is considered garbage.

### **What tools are available to monitor garbage collection?**

- **jconsole**
- **VisualVM**
- **jstat**
- **Java Flight Recorder**
- **GC logs** (`-Xlog:gc` or `-verbose:gc`)

### **What JVM options control garbage collection?**

-Xmx: Sets the maximum heap size
-Xms: Sets the initial heap size
-XX:MaxGCPauseMillis: Sets the maximum pause time for Garbage Collection
-XX:+UseConcMarkSweepGC: Uses Concurrent Mark Sweep Garbage Collection policy
-XX:+UseG1GC: Uses G1 Garbage Collection policy
-XX:NewRatio: Sets the ratio of the young generation to the old generation
-XX:SurvivorRatio: Sets the ratio of survivor space to Eden space

### **Can you stop or disable garbage collection in Java?**

No. GC is a core part of the JVM and **cannot be disabled**.
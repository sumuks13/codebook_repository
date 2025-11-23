---
{"publish":true,"title":"Memory Management","cssclasses":""}
---

## Definitions

**Garbage Collection**: Automatic memory management process identifying and deallocating unreferenced objects. Eliminates manual memory management burden.

**Heap Memory**: Runtime data area storing objects and arrays. Managed by GC. Shared across all threads.

**Stack Memory**: Stores method calls and local primitive variables. Thread-specific; automatically freed when method exits.

**Mark-and-Sweep**: Two-phase GC algorithm. Mark: identify in-use objects; Sweep: reclaim unreferenced memory.

**Generational Garbage Collection**: Divides heap into Young (short-lived objects) and Old (long-lived objects) generations for optimization.

**Object Eligibility**: Object eligible for GC when no references exist or only through GC roots.

---

## QnA

**When does an object become eligible for garbage collection?**

An object becomes eligible for GC when no active references to it exist from GC roots (stack locals, static variables, active threads). If an object is only reachable through other unreachable objects, it's also eligible. GC roots are the starting points for reachability analysis; anything not reachable from roots is garbage.

**Can you force garbage collection in Java?**

No, `System.gc()` or `Runtime.getRuntime().gc()` only requests GC—JVM decides whether to comply. You cannot force immediate garbage collection. The JVM uses GC algorithms and heap pressure to determine when to run GC. Relying on System.gc() is bad practice; design code assuming GC runs at unpredictable times.

**What's the difference between heap and stack?**

Stack stores method calls and local primitives; automatically freed when methods exit. Heap stores objects and arrays; managed by GC; freed when unreferenced. Stack is thread-specific with limited size; heap is shared and larger. Stack access is faster but limited; heap is slower but flexible for variable-size objects.

**What's generational garbage collection?**

Young generation handles short-lived objects with frequent minor GC; old generation handles long-lived objects with rare major GC. Most objects die young (Eden space). This generational approach reduces pause times because minor GC is fast, collecting mostly young objects. Only long-lived survivors are promoted to old generation.

**What happens with OutOfMemoryError?**

Thrown when heap is exhausted and GC cannot free sufficient memory. Application may crash or be terminated by JVM. This indicates either a memory leak (objects never dereferenced) or heap configured too small. Solutions: increase heap size, fix memory leaks, or optimize object creation.

**What's the finalize() method?**

Called by GC before reclaiming an object's memory. Deprecated as of Java 9 (use try-with-resources or AutoCloseable instead). Finalization is unpredictable—no guarantee when or if finalize() runs. Relying on finalize() for resource cleanup is unreliable; use explicit resource management.

---

## Code Examples

**Example - Object Eligibility:**
```java
public class GCExample {
    public static void main(String[] args) {
        Object obj = new Object();
        System.out.println("Object created");
        
        // Object eligible for GC (reference lost)
        obj = null;
        
        // Variable goes out of scope
        {
            Object temp = new Object();
        }  // temp eligible for GC here
        
        // Multiple references
        Object obj1 = new Object();
        Object obj2 = obj1;
        obj1 = null;  // obj2 still references
        obj2 = null;  // Now eligible
    }
}
```

**Example - OutOfMemoryError Prevention:**
```java
// Bad: Memory leak - keeps accumulating
List<byte[]> list = new ArrayList<>();
for (int i = 0; i < 1_000_000; i++) {
    list.add(new byte[1024]);
    // Eventually: OutOfMemoryError
}

// Better: Clear periodically
List<byte[]> boundedList = new ArrayList<>(1000);
for (int i = 0; i < 1_000_000; i++) {
    if (boundedList.size() >= 1000) {
        boundedList.clear();
    }
    boundedList.add(new byte[1024]);
}

// Best: Use appropriate collection
Queue<byte[]> queue = new LinkedList<>();
queue.offer(new byte[1024]);  // FIFO, naturally bounded
```

**Example - Resource Management (Best Practice):**
```java
// Modern way (automatic cleanup)
try (FileReader reader = new FileReader("file.txt")) {
    // Use reader
    // Automatically closed even if exception occurs
} catch (IOException e) {
    e.printStackTrace();
}
```

---

## Key Points

- **Reachability**: Only objects reachable from GC roots are kept. Circular references still eligible if unreachable from roots.
- **Young Generation Optimization**: Most objects die young; young GC frequent and fast; reduces full GC pauses.
- **GC Pause**: Stop-the-world event; application threads pause during GC. Larger heap = longer pauses.
- **Monitoring GC**: Use JVisualVM, JProfiler, or GC logs to identify memory leaks and optimize GC tuning.
- **Memory Leaks**: Possible via unclosed resources, static collections, listeners, circular references across boundaries.

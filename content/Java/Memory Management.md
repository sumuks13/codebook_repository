tags: [[Java/Java]], [[Java/Garbage Collection]]


![[attachments/Pasted image 20240419061735.png]]

### **What is memory management in Java?**

Memory management in Java refers to how the **Java Virtual Machine (JVM)** allocates and deallocates memory to objects and classes during a program's execution. It ensures that memory is used **efficiently**, and that **unused memory is reclaimed automatically**.

### **How is memory managed in Java?**

Java uses a **managed memory model**, which means:

- The **JVM** takes care of memory allocation
- **Garbage Collector (GC)** reclaims unused memory
- Developers focus on logic — no need for manual memory deallocation like in C/C++

### **What are the main areas of memory in the JVM?**

| Memory Area                       | Purpose                                              |
| --------------------------------- | ---------------------------------------------------- |
| **Heap**                          | Stores all objects and class instances               |
| **Stack**                         | Stores method calls, local variables, and references |
| **Method Area**                   | Stores class metadata, static variables, bytecode    |
| **Program Counter (PC) Register** | Holds the address of the current instruction         |
| **Native Method Stack**           | For native (non-Java) method calls                   |

### **What is the Java Heap memory?**

- Heap is the **runtime data area** from which memory for all Java class instances and arrays is allocated. 
- It is **shared among all threads**. 
- When you use a new keyword, the JVM creates an instance for the object in a heap. 
- While the reference of that object stores in the stack. 
- There exists only one heap for each running JVM process. 
- When heap becomes full, the garbage is collected.


### **What is the Stack memory in Java?**

Each thread gets its own **stack memory**, which stores:
- Local variables
- Method call frames
- References to objects in the heap

Stack memory is **faster** and **automatically cleaned** up when a method finishes.

### **What is the Method Area (MetaSpace)?**

- Used to store **class-level data**, like static variables, constants, and method metadata.
- In Java 8 and above, this is called **Metaspace** (replaces PermGen).
- It grows **dynamically** and is **not part of the heap**.

### **What is the Program Counter (PC) Register?**

Each thread has its own PC register which:
- Points to the **current executing instruction** in the method
- Is used to **restore execution** after method calls and returns

### **What is the Native Method Stack?**

It is also known as C stack. It is a stack for native code written in a language other than Java. Java Native Interface (JNI) calls the native stack. The performance of the native stack depends on the OS.

### **What are the different reference types for objects in Heap memory?**

1. Strong reference (we can create a strong reference by using new keyword).
2. Weak Reference
3. Soft Reference
4. Phantom Reference

```java
PhantomReference<StringBuilder> reference = new PhantomReference<>(new StringBuilder());
```

### **Explain about the Stack Frame**

Stack frame is a data structure that contains the thread's data. Thread data represents the state of the thread in the current method.

- When a method invokes, a new frame creates. It destroys the frame when the invocation of the method completes.
- Each frame contains own **Local Variable Array (LVA), Operand Stack (OS) and Frame Data (FD)**.
- Only one frame is active at any point in a given thread of control. This frame is called the current frame, and its method is known as the current method. The class of method is called the current class.
- The frame stops the current method, if its method invokes another method or if the method completes.
- The frame created by a thread is local to that thread and cannot be referenced by any other thread.

### **What causes `OutOfMemoryError` in Java?**

Common reasons:
- Heap is full and GC cannot free enough memory
- Creating too many objects without releasing references
- Infinite recursion or very deep call stack (causes `StackOverflowError`)

### **What are the types of memory errors in Java?**

|Error Type|Cause|
|---|---|
|`OutOfMemoryError`|Heap, Metaspace, or other memory is full|
|`StackOverflowError`|Method calls exceed the stack limit|
|`GC overhead limit`|Too much time spent on GC with little gain|

### **What is memory fragmentation?**

Memory fragmentation happens when **free memory is scattered** in small blocks.  
This can make it hard to allocate large objects even though there’s enough total free space.

Modern collectors like **G1 GC** handle fragmentation better by organizing memory into regions.

### **How do local variables and object references work in memory?**

- **Local variables** are stored in the **stack**
- If a local variable refers to an object, the **reference** is in the stack, but the **actual object is in the heap**        

### **Can objects be shared between threads? Where are they stored?**

Yes. **Objects are stored in heap**, which is **shared by all threads**.  
But local variables (in stack) are **thread-specific** and not shared.

### **What is escape analysis in Java?**

It’s a compiler optimization that determines **if an object is used only in a method**, and if so, the JVM may **allocate it on the stack instead of the heap** — making allocation faster and removing the need for GC.

### **What happens if a variable is declared but not used?**

It may still consume stack memory **until the method finishes**, but it can be **optimized out by the JVM** if not referenced.

### **Why there are no pointers in Java?**

- Java has references instead of pointers. 
- These references point to objects in memory. But there is no direct access to these memory locations. JVM is free to move the objects within VM memory. 
- The absence of pointers helps Java in managing memory and garbage collection effectively. Also it provides developers with convenience of not getting worried about memory allocation and de allocation.

### **If there are no pointers in Java, then why do we get NullPointerException?**

JVM uses pointers but programmers only see object references.
if an object reference points to null object, and we try to access a method or member variable on it, then we get NullPointerException.

tags: [[Java]], [[Garbage Collection]]

Process of allocation and de-allocation of objects is called Memory management.
Java does memory management automatically. Java uses an automatic memory management system called a **garbage collector**.

![[Pasted image 20240419061735.png]]

### Method Area

Method Area is a part of the heap memory which is shared among all the threads. It creates when the JVM starts up. It is used to store class structure, superclass name, interface name, and constructors. The JVM stores the following kinds of information in the method area:

- A Fully qualified name of a type (ex: String)
- The type's modifiers
- Type's direct superclass name
- A structured list of the fully qualified names of super interfaces.

### Heap Area

Heap stores the actual objects. It creates when the JVM starts up. The user can control the heap if needed. It can be of fixed or dynamic size. When you use a new keyword, the JVM creates an instance for the object in a heap. While the reference of that object stores in the stack. There exists only one heap for each running JVM process. When heap becomes full, the garbage is collected.

Heap is divided into the following parts:
- Young generation
- Survivor space
- Old generation
- Permanent generation
- Code Cache

Reference Types of Objects on Heap:
- **Strong reference:** It is very simple as we use it in our daily programming. Any object which has Strong reference attached to it is not eligible for garbage collection. We can create a strong reference by using new keyword.
- **Weak Reference:** It does not survive after the next garbage collection process. It is defined in **java.lang.ref.WeakReference** class.
- **Soft Reference:** It is collected when the application is running low on memory.
- **Phantom Reference:** The object which has only phantom reference pointing them can be collected whenever garbage collector wants to collect. `PhantomReference<StringBuilder> reference = new PhantomReference<>(new StringBuilder());'

### Stack Area

Stack Area generates when a thread creates. It can be of either fixed or dynamic size. The stack memory is allocated per thread. It is used to store data and partial results. It contains references to heap objects. It also holds the value itself rather than a reference to an object from the heap. The variables which are stored in the stack have certain visibility, called scope.

**Stack Frame:** Stack frame is a data structure that contains the thread's data. Thread data represents the state of the thread in the current method.

- When a method invokes, a new frame creates. It destroys the frame when the invocation of the method completes.
- Each frame contains own Local Variable Array (LVA), Operand Stack (OS), and Frame Data (FD).
- Only one frame (the frame for executing method) is active at any point in a given thread of control. This frame is called the current frame, and its method is known as the current method. The class of method is called the current class.
- The frame stops the current method, if its method invokes another method or if the method completes.
- The frame created by a thread is local to that thread and cannot be referenced by any other thread.

### Native Method Stack

It is also known as C stack. It is a stack for native code written in a language other than Java. Java Native Interface (JNI) calls the native stack. The performance of the native stack depends on the OS.

### PC Registers

Each thread has a Program Counter (PC) register associated with it. PC register stores the return address or a native pointer. It also contains the address of the JVM instructions currently being executed.
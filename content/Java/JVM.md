tags: [[Java]]

It contains classloader, memory area, execution engine etc.

![[Pasted image 20240414195225.png]]

## Classloader

Classloader is used to load class files. Whenever we run the java program, it is loaded first by the classloader. There are three built-in classloaders in Java.

1. **Bootstrap ClassLoader**: This is the first classloader which is the super class of Extension classloader. It loads the _rt.jar_ file which contains all class files like java.lang package classes, java.net package classes, java.util package classes, java.io package classes, java.sql package classes etc.

2. **Extension ClassLoader**: This is the parent classloader of System classloader. It loads the jar files located inside _$JAVA_HOME/jre/lib/ext_ directory.

3. **System/Application ClassLoader**: It loads the class files from classpath. By default, classpath is set to current directory. You can change the classpath using "-cp" or "-classpath" switch. It is also known as Application classloader.

#### Class(Method) Area

Class(Method) Area stores structures such as the runtime constant pool, fields and methods data, the code for methods.

#### 1) Heap

It is the runtime data area in which objects are allocated memory.

#### 2) Stack

Java Stack stores frames. It holds local variables and partial results, and plays a part in method invocation and return.
A new frame is created each time a method is invoked. A frame is destroyed when its method invocation completes.

#### 3) Program Counter Register

PC (program counter) register contains the address of the Java virtual machine instruction currently being executed.

#### 4) Native Method Stack

It contains all the native methods used in the application.

## Execution Engine

It Contains interpreter and JIT compiler.

Java compiler converts Java code into bytecode. Bytecode is a platform-independent. 
The bytecode is then interpreted by JVM into machine code which runs on the computer's operating system.
The JIT compiler compiles the bytecode into machine code as it is being interpreted.
When the same code is called again, it will not interpret again as it is already compiled.
## Java Native Interface

Java Native Interface (JNI) is a framework which provides an interface to communicate with another application written in another language
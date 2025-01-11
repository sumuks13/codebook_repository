tags : [[Java]]
## 1. **What is the difference between JDK , JRE and JVM?** 

JDK stands for Java Development Kit. It contains the tools and libraries for development of Java programs along with JRE and JVM. It also contains compilers and debuggers.

JRE stands for Java Runtime Environment. It provides libraries and JVM that is required to run a Java program.

JVM is an abstract m/c that executes java bytecode. It is platform dependent. Converts bytecode into m/c code for a specific platform. Its implementation is known as JRE.

![[Pasted image 20240414194835.png]]
## 2. What is JIT Compiler?

Just In Time Compiler is used for performance improvement in Java. It is compilation done at execution time rather than earlier.

## 3. Why is Java 'write once - run anywhere' language?

You can write Java in windows and compile it in windows platform.
The jar file can then be run on a Linux m/c as is.

## 4. What is static typing, dynamic typing?

In static typed languages data types are defined at compile time and cannot be changed during runtime.
In dynamic typed languages, a variable can change its data type.

## 5. What is strongly types and weakly typed languages?

Strongly typed languages do not allow type flexibility. i.e., adding int with a String variable, dividing float with int.

## 6. In Java what are the default values for variables?

default value is NULL for all reference based data types.

## 7. Let say, we run a java class without passing any arguments. What will be the value of String array of arguments in Main method? 

By default, the value of String array of arguments is empty in Java. It is not null.

## 8. What is the difference between byte and char data types in Java?

Both are numeric data types in Java.  
A byte can store raw binary data where as a char stores characters or text data.
Byte values range from -128 to 127. 
1 byte = 8 bits, 1 char = 16 bits.

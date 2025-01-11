tags: [[Java]], [[Interview]]

### Explain the expected output of the following code segment?
``` run-java

public class PrintTest {
	public static void main(String[] args) {
		System.out.println(100 + 100 +"Simplilearn");
		System.out.println("Simplilearn" + 100 + 100);
	}
}
```


### 1. Why is Java a platform independent language?

Java code is compiled and converted into platform independent byte code which can run on any machine which has JRE installed

### 2. Why is Java not a pure object oriented language?

Java supports primitive data types - byte, Boolean, char, short, int, float, long, and double and hence it is not a pure [**object oriented language**](https://www.interviewbit.com/oops-interview-questions/)

### 3. Difference between Heap and Stack Memory in java. And how java utilizes this.

- In Java, memory management is handled by the JVM, which divides memory into two parts: stack memory and heap memory. 
- all the methods and variables are stored in stack memory while all the objects created during run time are stored in heap memory. 
- Stack memory is faster but also smaller than heap.

Example- **Consider the below java program**:

```java
class Main {
   public void printArray(int[] array){
       for(int i : array)
           System.out.println(i);
   }
   public static void main(String args[]) {
       int[] array = new int[10];
       printArray(array);
   }
}
```

For this java program. The stack and heap memory occupied by java is -

![](https://d3n0h9tb65y8q.cloudfront.net/public_assets/assets/000/003/020/original/stack_and_heap_memory.png?1648813762)


### 4. Why does Java not make use of pointers?

Since users can directly access the memory with the help of pointers, it leads a compromise in security, generally unsafe.

### 5. What do you understand by an instance variable and a local variable?

- Instance variable are class variables - declared outside methods and can be accessed by all methods of the class
- Local variables are method variables - declared inside methods or blocks, have a limited scope only throughout the method/block.

### 6. What do you mean by data encapsulation?

- Data Encapsulation is an Object-Oriented Programming concept of hiding the data attributes (private access modifier) and their behaviours in a single unit.

![[Pasted image 20230920060841.png]]

### 7. Tell us something about JIT compiler.



### 8. What are the differences between JVM, JRE and JDK in Java?

|Criteria|JDK|JRE|JVM|
|---|---|---|---|
|Abbreviation|Java Development Kit|Java Runtime Environment|Java Virtual Machine|
|Definition|JDK is a complete software development kit for developing Java applications. It comprises JRE, JavaDoc, compiler, debuggers, etc.|JRE is a software package providing Java class libraries, JVM and all the required components to run the Java applications.|JVM is a platform-dependent, abstract machine comprising of 3 specifications - document describing the JVM implementation requirements, computer program meeting the JVM requirements and instance object for executing the Java byte code and provide the runtime environment for execution.|
|Main Purpose|JDK is mainly used for code development and execution.|JRE is mainly used for environment creation to execute the code.|JVM provides specifications for all the implementations to JRE.|
|Tools provided|JDK provides tools like compiler, debuggers, etc for code development|JRE provides libraries and classes required by JVM to run the program.|JVM does not include any tools, but instead, it provides the specification for implementation.|
|Summary|JDK = (JRE) + Development tools|JRE = (JVM) + Libraries to execute the application|JVM = Runtime environment to execute Java byte code.|

### 9. Can you tell the difference between equals() method and equality operator (= =) in Java?

|equals()|= =|
|---|---|
|This is a method defined in the Object class.|It is a binary operator in Java.|
|The .equals() Method is present in the Object class, so we can override our custom .equals() method in the custom class, for objects comparison.|It cannot be modified. They always compare the HashCode.|
|This method is used for checking the equality of contents between two objects as per the specified business logic.|This operator is used for comparing addresses (or references), i.e checks if both the objects are pointing to the same memory location.|

### 10. Briefly explain the concept of constructor overloading

Constructor overloading is the process of creating multiple constructors in the class consisting of the same name with a difference in the constructor parameters. Depending upon the number of parameters and their corresponding types, distinguishing of the different types of constructors is done by the compiler.

![[Pasted image 20231010052432.png]]

### 11. Define Copy constructor in java.

Copy Constructor is the constructor used when we want to initialize the value to the new object from the old object of the same class. 

```java
class InterviewBit{
   String department;
   String service;
   InterviewBit(InterviewBit ib){
       this.departments = ib.departments;
       this.services = ib.services;
   }
}
```

Here we are initializing the new object value from the old object value in the constructor. Although, this can also be achieved with the help of object cloning.

### 12. Can the main method be Overloaded?

Yes, It is possible to overload the main method. We can create as many overloaded main methods we want. However, JVM has a predefined calling method that JVM will only call the main method with the definition of - 

```java
public static void main(string[] args)
```

## 13. Is it possible to execute a program without defining a main() method? 

No, with Java 7 onwards, you need a main() method to execute a program. In earlier versions of Java, there was a workaround available to use static blocks for execution. But now this gap has been closed.

## 14. Does Java allow virtual functions? 

Yes. All instance methods in Java are virtual functions by default. Only class methods and private instance methods are not virtual methods in Java. All functions which can be overridden are virtual functions.
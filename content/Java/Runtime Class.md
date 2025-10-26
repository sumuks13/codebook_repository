tags: [[Java/Java]]

Java Runtime class is used to interact with java runtime environment. Java Runtime class provides methods to execute a process, invoke GC, get total and free memory etc. There is only one instance of java.lang.Runtime class is available for one java application.

The Runtime.getRuntime() method returns the singleton instance of Runtime class.

Java Runtime exec() method

```java
public class Runtime1{  
 public static void main(String args[])throws Exception{  
  Runtime.getRuntime().exec("notepad");
  //will open a new notepad  
 }  
}  
```

Java Runtime freeMemory() and totalMemory() method

In the given program, after creating 10000 instance, free memory will be less than the previous free memory. But after gc() call, you will get more free memory.

```java
public class MemoryTest{  
 public static void main(String args[])throws Exception{  
  Runtime r=Runtime.getRuntime();  
  System.out.println("Total Memory: "+r.totalMemory());  
  System.out.println("Free Memory: "+r.freeMemory());  
    
  for(int i=0;i<10000;i++){  
   new MemoryTest();  
  }  
  System.out.println("After creating 10000 instance, Free Memory: "+r.freeMemory());  
  System.gc();  
  System.out.println("After gc(), Free Memory: "+r.freeMemory());  
 }  
}  
```


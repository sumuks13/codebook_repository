The **static keyword** in [Java](https://www.javatpoint.com/java-tutorial) is used for memory management mainly. It makes your program **memory efficient** (i.e., it saves memory)

The static can be:

1. Variable (also known as a class variable)
2. Method (also known as a class method)
3. Block
4. Nested class

#### 1. In Java, why do we use static variable? 

Whenever we want to have a common property for all objects of a class, we use a class level variable (static variable).
This variable is loaded in memory only once at the time of class loading. So it saves memory, since it is not defined per object.

#### 2. Why it is not a good practice to create static variables in Java?

Static variables are common to all the objects of a class. 
Any code that uses static variable can be in any state. It can be within a new object or at a class level. 
So the scope of static variable is open ended in a Java class. If we want tighter control on scope, then variables should be created at the object creation level. 
**Also defining static variables is not a good practice because they go against the principles of Object Oriented Programming**

#### 3. What is the purpose of static method in Java? 

Java provides the feature of static method to create behavior at the class level. 
The static method is common to all the objects of a class. We do not need to create any object of a class to call a static method. 
So it provides convenience of not creating an object for calling it. Also a static method can access and modify static data members.

#### 4. Why do we mark main method as static in Java? 

The main method in Java is marked as static, so that JVM can call it to start the program. If main method is not static, then which constructor will be called by Java process? 
As such it is a known as convention to mark main method static in Java. 
But if we remove the static, then there will be ambiguity. Java process may not know which method of a class to call to start the program. So this convention helps in Java process to identify the starting code for a program in class that is passed as an argument to java process

#### 5. In what scenario do we use a static block? 

At times, there is a class that has static member variables. These variables need some complicated initialization. At this time static block helps to initialize complex static member variable initialization. 
The static block is executed even before the execution of main. Sometimes, we can also replace static block with a static method of class

#### 6. What happens when static modifier is not mentioned in the signature of main method? 

As per Java specification, main method has to be marked as static. It needs only one argument that is an array of String. A program can compile with a non-static method. But on execution it will give NoSuchMethodError

#### 7. What is the difference between static method and instance method in Java? 

Often, there is a need to define a behavior for a class that is not dependent on member variables of an object. Such behavior is captured in a static method. 
If there is a behavior dependent upon the member variables of an object, then we do not mark it static, it remains as instance method. 
To call as static method, we do not need to create an object. We just call it with class name. 
But to call an instance method, we need to create/get an object first. 
Instance member variables cannot be accessed by a static method. 
But an instance method can call both instance variables and static variables
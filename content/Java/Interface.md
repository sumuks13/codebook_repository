tags : [[Java]]

An **interface in Java** is a blueprint of a class. It has static constants and abstract methods.
There can be only abstract methods in the Java interface, not method body. It is used to achieve abstraction and multiple [inheritance in Java](https://www.javatpoint.com/inheritance-in-java).

Java Interface also **represents the IS-A relationship**.

- It is used to achieve abstraction.
- By interface, we can support the functionality of multiple inheritance.
- It can be used to achieve loose coupling.

Interface fields are public, static and final by default, and the methods are public and abstract.

![interface in java](https://static.javatpoint.com/images/interface.png)

![[Pasted image 20240416091331.png]]


#### Multiple inheritance in Java by interface

If a class implements multiple interfaces, or an interface extends multiple interfaces, it is known as multiple inheritance.

![multiple inheritance in java](https://static.javatpoint.com/images/core/multipleinheritance.jpg)

Since Java 8, we can have static methods and default method body in interface.

```java
interface Drawable{  
 void draw();  
 default void msg(){System.out.println("default method");}  
}
```

#### What is marker or tagged interface?

An interface which has no member is known as a marker or tagged interface, for example, [Serializable](https://www.javatpoint.com/serialization-in-java), Cloneable, Remote, etc. They are used to provide some essential information to the JVM so that JVM may perform some useful operation.

```java
public interface Serializable{  
}
```

| Abstract class                                                                                 | Interface                                                                                                    |
| ---------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------ |
| 1) Abstract class can **have abstract and non-abstract** methods.                              | Interface can have **only abstract** methods. Since Java 8, it can have **default and static methods** also. |
| 2) Abstract class **doesn't support multiple inheritance**.                                    | Interface **supports multiple inheritance**.                                                                 |
| 3) Abstract class **can have final, non-final, static and non-static variables**.              | Interface has **only static and final variables**.                                                           |
| 4) Abstract class **can provide the implementation of interface**.                             | Interface **can't provide the implementation of abstract class**.                                            |
| 5) The **abstract keyword** is used to declare abstract class.                                 | The **interface keyword** is used to declare interface.                                                      |
| 6) An **abstract class** can extend another Java class and implement multiple Java interfaces. | An **interface** can extend another Java interface only.                                                     |
| 7) An **abstract class** can be extended using keyword "extends".                              | An **interface** can be implemented using keyword "implements".                                              |
| 8) A Java **abstract class** can have class members like private, protected, etc.              | Members of a Java interface are public by default.                                                           |
| 9)**Example:**  <br>public abstract class Shape{  <br>public abstract void draw();  <br>}      | **Example:**  <br>public interface Drawable{  <br>void draw();  <br>}                                        |

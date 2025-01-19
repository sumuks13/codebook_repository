tags: [[OOPS]]
#### 1. What is the purpose of ‘this’ keyword in java? 

‘this’ keyword refers to current instance of the object. 
useful for differentiating between instance variables and local variables. 
It can be used to call constructors.

#### 2. Explain the concept of Inheritance? 

By using Inheritance, we can put the common behavior and characteristics in a base class which also known as super class. And then all the objects with common behavior inherit from this base class. 
It is also represented by IS-A relationship. 
Inheritance promotes code reuse, method overriding and polymorphism.

#### 3. Which class in Java is superclass of every other class? 

Object class.

#### 4. Why Java does not support multiple inheritance? 

Multiple Inheritance means that a class can inherit behavior from two or more parent classes. If both the parent classes may have the same method, it leads to ambiguity. This is the main reason for Java not supporting Multiple Inheritance in implementation.
But you can implement multiple interfaces in Java.

#### 5. In OOPS, what is meant by composition?

Composition is also known as “has-a” relationship.
E.g. Class Car has a steering wheel. 
If a class holds the instance of another class, then it is called composition.

#### 6. What is meant by aggregation?

Aggregation is a type of association relation in which objects can exists irrespective of each other.
A student can be related to Library. A student can still exist without a library (Weaker Composition).
But an Order cannot exist without a Customer (Composition)

#### 7. Why there are no pointers in Java?

Java has references instead of pointers. These references point to objects in memory. 
But there is no direct access to these memory locations. JVM is free to move the objects within VM memory. T
he absence of pointers helps Java in managing memory and garbage collection effectively. Also it provides developers with convenience of not getting worried about memory allocation and de allocation.

#### 8. If there are no pointers in Java, then why do we get NullPointerException?

JVM uses pointers but programmers only see object references.
if an object reference points to null object, and we try to access a method or member variable on it, then we get NullPointerException.

#### 9. What is the purpose of ‘super’ keyword in java? 

It refers to parent class of an object. By using ‘super’ we can call a method of parent class from the method of a child class. We can also call the constructor of a parent class from the constructor of a child class by using ‘super’ keyword

#### 10. Is it possible to use this() and super() both in same constructor?

No, As per Java specification, super() or this() must be the first statement in a constructor.

#### 11. What is the meaning of object cloning in Java? 

Object.clone() method is used for creating an exact copy of the object in Java. 
It acts like a copy constructor. It creates and returns a copy of the object, with the same class and with all the fields having same values as of the original object. 
One disadvantage of cloning is that the return type is an Object. It has to be explicitly cast to actual type.

## Types of inheritance in java

On the basis of class, there can be three types of inheritance in java: single, multilevel and hierarchical.
In java programming, multiple and hybrid inheritance is supported through interface only.

## Aggregation in Java

If a class has an entity reference, it is known as Aggregation. Aggregation represents HAS-A relationship.

```java
class Employee{  
 int id;  
 String name;  
 Address address;//Address is a class  
} 
```

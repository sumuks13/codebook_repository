tags : [[Java/Java]], [[Java/Inheritance]], [[Java/Polymorphism]], [[Java/Abstraction]], [[Encapsulation]]

The full form of OOPS is Object-Oriented Programming System. 

It is a programming paradigm based on the concept of “objects”, which can contain data and code to manipulate that data. The main aim of OOP is to bind together the data and the functions that operate on them so that no other part of the code can access this data except that function. 

The key concepts of OOP include Class, Objects, Data Abstraction, Encapsulation, Inheritance, Polymorphism, Dynamic Binding, and Message Passing.

Apart from Object-Oriented Programming (OOP), there are several other programming paradigms:

**Imperative Programming**: This is one of the oldest programming paradigms. It works by changing the program state through assignment statements. It performs tasks step by step by changing state. Examples of languages that support this paradigm include C and Fortran.

**Procedural Programming**: This paradigm emphasizes on procedure in terms of the underlying machine model2. It has the ability to reuse the code. Examples of languages that support this paradigm include C, C++, and Java.

**Functional Programming**: This paradigm emphasizes on pure functions. It avoids changing-state and mutable data. Haskell and Lisp are examples of languages that support this paradigm.

**Declarative Programming**: In this paradigm, the focus is on what needs to be done, rather than how to do it1. It hides complexity and is used in databases, regular expressions, and configuration management tools.

#### 1. What are the main principles of Object Oriented Programming?

Abstraction
Encapsulation 
Inheritance 
Polymorphism

#### 2. What is the difference between Object Oriented Programming language and Object Based Programming language?

Object Oriented Programming languages follow concepts of OOPS - Java
Object Based Programming languages follow some features of OOPS but not provide support for Polymorphism and Inheritance - JavaScript
Object Based Programming languages provide support for Objects and you can build objects from constructor. They languages also support Encapsulation. These are also known as Prototype-oriented languages.

#### 3. Why do we need constructor in Java?

A constructor is a piece of code similar to a method. It is used to create an object and set the initial state of the object.
A constructor is a special function that has same name as class name. Without a constructor, there is no other way to create an object.
By default, Java provides a default constructor for every object (NoArgsConstructor)

#### 4. What is the value returned by Constructor in Java? 

When we call a constructor in Java, it returns the object created by it. That is how we create new objects in Java.

#### 5. Can we inherit a Constructor? 

No, Java does not support inheritance of constructor

#### 6. Why constructors cannot be final, static, or abstract in Java? 

**FINAL** - If we set a method as final it means we do not want any class to override it. But the constructor cannot be overridden. So there is no use of marking it final. 
**ABSTRACT** - If we set a method as abstract it means that it has no body and it should be implemented in a child class. But the constructor is called implicitly when the new keyword is used. Therefore it needs a body. 
**STATIC** - If we set a method as static it means that it belongs to the class, but not a particular object. The constructor is always called to initialize an object. Therefore, there is no use of marking constructor static.
## Method Overloading

#### 1. What is the other name of Method Overloading? 

 Static Polymorphism.

#### 2. How will you implement method overloading in Java? 

In Java, a class can have methods with same name but different arguments. It is called Method Overloading. 

To implement method overloading we have to create two methods with same name in a class and do one/more of the following: 
1. Different number of parameters
2. Different data type of parameters
3. Different sequence of data type of parameters

#### 3. Why it is not possible to do method overloading by changing return type of method in java? 

If we change the return type of overloaded methods then it will lead to ambiguous behavior. How will clients know which method will return what type. Due to this different return type are not allowed in overloaded methods.

#### 4. Is it allowed to overload main() method in Java? 

Yes, Java allows users to create many methods with same name ‘main’. But only public static void main(String[] args) method is used for execution

---
## Method Overriding

#### 1. How do we implement method overriding in Java? 

To override a method, we just provide a new implementation of a method with same name in subclass. So there will be at least two implementations of the method with same name. One implementation is in parent class. And another implementation is in child class

#### 2. Are we allowed to override a static method in Java? 

No. Java does not allow overriding a static method. If you create a static method with same name in subclass, then it is a new method, not an overridden method

To override a method, you need an instance of a class. Static method is not associated with any instance of the class. So the concept of overriding does not apply here

#### 3. Is it allowed to override an overloaded method? 

Yes. You can override an overloaded method in Java

#### 4. What is the difference between method overloading and method overriding in Java? 

 1. Method overloading is static polymorphism. Method overriding is runtime polymorphism.
 2. Method overloading occurs within the same class. Method overriding happens in two classes with hierarchy relationship.
 3. Parameters must be different in method overloading. Parameters must be same in method overriding. 
 4.   Method overloading is a compile time concept. Method overriding is a runtime concept

#### 5. What is meant by covariant return type in Java? 

A covariant return type of a method is one that can be replaced by a "narrower" type when the method is overridden in a subclass. 
Let say class B is child of class A. There is a get() method in class A as well as class B. get() method of class A can return an instance of A, and get() method of class B return an instance of B. Here class B overrides get() method, but the return type is different. 
From Java 5 onwards, a child class can override a method of parent class and the child class method can return an object that is child of object return by parent class method.


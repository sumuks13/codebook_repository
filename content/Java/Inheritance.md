tags: [[Java/OOPS]]

### **Explain the concept of Inheritance**

- By using Inheritance, we can put the common behavior and characteristics in a base class which also known as super class. And then all the objects with common behavior inherit from this base class. 
- It is also represented by IS-A relationship. 
- Inheritance promotes code reuse, method overriding and polymorphism.

### **Which class in Java is superclass of every other class?**

Object class.

### **What is the purpose of ‘this’ keyword in java?**

- ‘this’ keyword refers to current instance of the object. 
- useful for differentiating between instance variables and local variables. 
- It can be used to call constructors.

### **Why Java does not support multiple inheritance?**

- Multiple Inheritance means that a class can inherit behavior from two or more parent classes. 
- If both the parent classes may have the same method, it leads to ambiguity. 
- This is the main reason for Java not supporting Multiple Inheritance in implementation.
- But you can implement multiple interfaces in Java.

### **In OOPS, what is meant by composition?**

- Composition is also known as strong “has-a” relationship.
- E.g. Class Car has a steering wheel. 
- If a class holds the instance of another class, then it is called composition.
- In composition, the contained object is an integral part of the container object and cannot exist independently.

### **What is meant by aggregation?**

- Aggregation is also known as weak “has-a” relationship.
- A student can be related to Library. A student can still exist without a library.
- Aggregation is a type of association relation in which objects can exists irrespective of each other.

| Feature      | Aggregation                               | Composition                                  |
| ------------ | ----------------------------------------- | -------------------------------------------- |
| Relationship | Weak "has-a"                              | Strong "has-a" (part-of)                     |
| Independence | Contained object can exist independently. | Contained object cannot exist independently. |
| Lifecycle    | Independent lifecycles.                   | Dependent lifecycles.                        |
| Ownership    | No strong ownership implied.              | Strong ownership implied.                    |

### **What is the purpose of ‘super’ keyword in java?**

- It refers to parent class of an object. 
- By using ‘super’ we can call a method of parent class from the method of a child class. 
- We can also call the constructor of a parent class from the constructor of a child class by using ‘super’ keyword

### **Is it possible to use this() and super() both in same constructor?**

No, As per Java specification, super() **or** this() must be the first statement in a constructor.

### **What are the types of inheritance in java**

On the basis of class, there can be three types of inheritance in java: **single**, **multilevel** and **hierarchical**.
In java programming, **multiple** and **hybrid** inheritance are supported through interface only.

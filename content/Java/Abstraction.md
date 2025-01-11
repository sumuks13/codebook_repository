Abstraction lets you focus on what the [object](https://www.javatpoint.com/object-and-class-in-java) does instead of how it does it.
	
A class which is declared as abstract is known as an **abstract class**. It can have abstract and non-abstract methods. It needs to be extended and its method implemented. It cannot be instantiated (Abstract classes cannot have objects)

- An abstract class must be declared with an abstract keyword.
- It can have abstract and non-abstract methods.
- It cannot be instantiated.
- It can have [constructors](https://www.javatpoint.com/java-constructor) and static methods also.
- It can have final methods which will force the subclass not to change the body of the method.

### Abstract Method in Java

A method which is declared as abstract and does not have implementation is known as an abstract method.

1. **Common Base Class**:
    - Abstract classes provide a common base for related classes.
    - When multiple classes share common properties or methods, you can create an abstract class to encapsulate that shared functionality.
    - Example: If you have different shapes (like circles, rectangles, and triangles), you can create an abstract `Shape` class with common methods like `area()` and `perimeter()`.
2. **Partial API Definition**:
    - Abstract classes allow you to define an API with some methods already implemented.
    - Subclasses can then extend the abstract class and provide their own implementations for the remaining methods.
    - Example: An abstract `Animal` class might have a method `makeSound()`, which subclasses like `Dog` and `Cat` can override.
3. **Code Reuse**:
    - Abstract classes promote code reuse.
    - You can define common behavior once in the abstract class and reuse it across multiple subclasses.
    - Example: An abstract `Vehicle` class might have methods like `startEngine()` and `stopEngine()`, which various vehicle types (cars, bikes, trucks) can inherit.
4. **Enforcing Method Implementation**:
    - Abstract classes force subclasses to implement specific methods.
    - If a class extends an abstract class, it must provide concrete implementations for all abstract methods.

#### 1. What is Abstraction in Object Oriented programming? 

Abstraction is the process of hiding certain implementation details of an object and showing only essential features of the object to outside world.
It helps us in focusing on the interface that we share with the outside world.

#### 2. How is Abstraction different from Encapsulation? 

Abstraction happens at class level design. It results in hiding the implementation details. 
Encapsulation is also known as “Information Hiding”. An example of encapsulation is marking the member variables private and providing getter and setter for these member variables

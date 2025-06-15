tags: [[OOPS]]

A class which is declared as abstract is known as an **abstract class**. It can have abstract and non-abstract methods. It needs to be extended and its method implemented. It cannot be instantiated 

#### **Abstract Method in Java**

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

#### **What is Abstraction in Object Oriented programming?**

- Abstraction is a programming concept where implementation details are hidden, and only essential behavior is shown to the user.  
- In simple terms, it tells you **what** an object does, not **how** it does it.

#### **Why is abstraction important in Java?**

- To reduce complexity by hiding internal logic
- To define a clear and reusable structure
- To make the code more maintainable and scalable

#### **How is abstraction achieved in Java?**

- Using **abstract classes**
- Using **interfaces**

#### **What is an abstract class?**

- An abstract class is a class which is declared with an abstract keyword.
- It can have abstract and non-abstract methods.
- It cannot be instantiated.
- It can have constructors and static methods also.
- It can have final methods which will force the subclass not to change the body of the method.

#### **What is an abstract method?**

A method declared without a body using the `abstract` keyword. It must be implemented by any concrete subclass.

```java
abstract void start();
```

#### **Can an abstract class have implemented methods?**

Yes. Abstract classes can have both implemented and unimplemented methods.

#### **Can we create an object of an abstract class?**

No. Abstract classes cannot be directly instantiated. But they can be used as reference types to assign an object of any concrete subclass. 

#### **Can an abstract class have constructors?**

Yes. They are used to initialize fields when an instance of a subclass is created.

#### **What is an interface in Java?**

An interface defines a behavior that a class must implement.  
- Before Java 8: all methods in an interface had to be abstract.  
- Since Java 8: interfaces can have `default` and `static` methods too.

#### **Difference between abstract class and interface?**

| Feature      | Abstract Class               | Interface                                   |
| ------------ | ---------------------------- | ------------------------------------------- |
| Methods      | Abstract + Concrete + static | Abstract (Java 7), default/static (Java 8+) |
| Constructors | Yes                          | No                                          |
| Variables    | Instance/static/any access   | `public static final` only                  |
| Inheritance  | Single inheritance           | Multiple inheritance                        |
| Use case     | Common base class with logic | Common contract across unrelated classes    |

#### **When should you use an abstract class instead of an interface?**

Use an abstract class when:
- You want to provide shared code for all subclasses
- You need constructors or non-final variables
- Classes are closely related by behavior

Use an interface when:
- You want to define a contract across unrelated classes
- You need multiple inheritance

#### **Can a class extend an abstract class and implement an interface at the same time?**

Yes. A class can extend one abstract class and implement multiple interfaces.

```java
class MyClass extends BaseClass implements MyInterface{
	// override methods 
}
```

#### **Can an abstract class implement an interface?**

Yes. It may implement some or all of the interface methods.  
Unimplemented methods must be handled by the subclass.

#### **Can abstract methods be static, final, or private?**

No, because:
- `abstract` and `static` conflict (static methods can’t be overridden)
- `abstract` and `final` conflict (final means no overriding)
- `abstract` and `private` conflict (private methods are not visible to subclasses)

#### **Can we declare an abstract class without any abstract methods?**

Yes. It just means the class cannot be instantiated. Useful when forcing inheritance or partial implementation.

#### **What happens if a class doesn't implement all abstract methods?**

That class must also be declared `abstract`. Otherwise, it results in a compilation error.

#### **Can an interface extend another interface?**

Yes. Interfaces can extend one or more other interfaces.

```java
interface B extends A { }
```

#### **Can we achieve multiple inheritance using abstraction in Java?**

Yes. A class can implement multiple **interfaces**, allowing it to inherit behavior from multiple sources.

```java
class A implements Interface1, Interface2 { }
```

#### **How is abstraction different from encapsulation?**

- **Abstraction** hides implementation logic
- **Encapsulation** hides internal state using private fields and public methods

#### **Can an abstract class have a `main` method?**

Yes. It can be used to test static blocks or class behavior.

#### **Give a real-world example of abstraction.**

- When you drive a car, you interact with essential features like the **steering wheel, accelerator pedal, and brake pedal**. 
- You use these components to control the car's movement without needing to understand the details of how the engine works, how fuel combustion occurs, or how the braking system brings the car to a halt. 
- The complex internal mechanisms (implementation details) are hidden, and only the necessary controls (essential features/interface) are exposed to the driver.
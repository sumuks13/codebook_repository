---
{"publish":true,"title":"Inheritance","cssclasses":""}
---

## Definitions

**Inheritance**: Mechanism allowing a class (subclass) to inherit properties and methods from another class (superclass). Enables code reuse and hierarchical relationships.

**Superclass/Parent Class**: Class whose properties and methods are inherited by another class.

**Subclass/Child Class**: Class that inherits from superclass using `extends` keyword.

**super keyword**: Reference to parent class members. Used to call parent constructor with `super()` or access overridden methods/variables.

**IS-A Relationship**: Inheritance represents "is-a" relationship (Dog is-an Animal).

---

## QnA

**What is the `super` keyword used for?**

The `super` keyword provides access to parent class members from within a subclass. It's primarily used in two ways: calling parent constructors with `super()` to initialize parent state, and accessing overridden methods or variables from parent class using `super.methodName()` or `super.variableName`. This is particularly useful when you override a parent method but still want to execute the parent's implementation first.

**Can you have multiple inheritance in Java?**

No, Java enforces single inheritance for classes to avoid the diamond problem (ambiguity when two parent classes have the same method). However, Java achieves multiple inheritance of type through interfaces, which allows a class to implement multiple interfaces and gain their method contracts.

**Does a subclass automatically inherit all parent members?**

Yes, all public and protected members are inherited by the subclass. Private members are not directly accessible in the subclass, but they still exist and can be manipulated through public getter and setter methods from the parent class. This encapsulation is intentional to maintain data protection.

**What happens if child and parent have the same variable?**

The child variable shadows the parent variable, meaning the child's version takes precedence in the subclass context. To access the parent's version, use `super.variableName`. This shadowing is generally discouraged as it can cause confusion, but understanding it is important for interviews.

**Can a subclass have its own constructor?**

Yes, subclasses can have multiple constructors with different parameter lists. The first line of a subclass constructor should explicitly call `super()` to initialize parent state, or if not specified, Java implicitly calls the parent's default constructor. If the parent has no default constructor, you must explicitly call `super(args)` with appropriate arguments.

**What is constructor chaining?**

Constructor chaining is the practice of calling one constructor from another constructor within the same class or parent class. Use `super()` to call a parent class constructor and `this()` to call another constructor in the same class. This reduces code duplication and ensures proper initialization of all class members in a hierarchy.

---

## Code Examples

**Example - Basic Inheritance:**
```java
// Parent class
class Animal {
    protected String name;
    protected int age;
    
    public Animal(String name, int age) {
        this.name = name;
        this.age = age;
    }
    
    public void displayInfo() {
        System.out.println("Name: " + name + ", Age: " + age);
    }
    
    public void sound() {
        System.out.println("Some animal sound");
    }
}

// Child class
class Dog extends Animal {
    private String breed;
    
    public Dog(String name, int age, String breed) {
        super(name, age);      // Call parent constructor
        this.breed = breed;
    }
    
    public void displayInfo() {
        super.displayInfo();   // Call parent method
        System.out.println("Breed: " + breed);
    }
    
    @Override
    public void sound() {
        System.out.println("Dog barks");
    }
}

// Usage
Dog dog = new Dog("Buddy", 3, "Labrador");
dog.displayInfo();
dog.sound();
```

**Example - Inheritance Hierarchy:**
```java
class Vehicle {
    protected String brand;
    
    public Vehicle(String brand) {
        this.brand = brand;
    }
}

class Car extends Vehicle {
    private int wheels = 4;
    
    public Car(String brand) {
        super(brand);
    }
}

class ElectricCar extends Car {
    private double batteryCapacity;
    
    public ElectricCar(String brand, double battery) {
        super(brand);
        this.batteryCapacity = battery;
    }
}
```

**Example - super() vs this():**
```java
class Parent {
    public Parent() {
        System.out.println("Parent constructor");
    }
    
    public Parent(String name) {
        System.out.println("Parent with name: " + name);
    }
}

class Child extends Parent {
    public Child() {
        super();  // Calls parent default constructor
        System.out.println("Child constructor");
    }
    
    public Child(String name) {
        super(name);  // Calls parent with parameter
        System.out.println("Child with name: " + name);
    }
    
    public Child(String name, int age) {
        this(name);  // Calls another constructor in Child
        System.out.println("Child with age: " + age);
    }
}
```

**Example - Accessing Parent Members:**
```java
class Parent {
    public void method1() {
        System.out.println("Parent method1");
    }
    
    public static void staticMethod() {
        System.out.println("Parent static method");
    }
}

class Child extends Parent {
    @Override
    public void method1() {
        super.method1();  // Call parent's overridden method
        System.out.println("Child method1");
    }
}

// Usage
Child child = new Child();
child.method1();  // Calls overridden child method
```

---

## Key Points

- **Constructor Chain**: Every constructor implicitly calls `super()` if first line not specified, invoking the default parent constructor. If parent has no default constructor, you must explicitly specify `super(args)`.
- **protected vs private**: Protected members are accessible in subclasses and same package; private members are not accessible even in subclasses, enforcing strict encapsulation.
- **Shadowing**: If child has same variable as parent, child's version shadows parent's. Use `super` to access parent's version. This is generally discouraged as it reduces code clarity.
- **super Must Be First**: In any constructor, `super()` or `this()` must be the first executable statement before any other code to ensure proper initialization.
- **Single Inheritance**: Java enforces single inheritance for classes to avoid the diamond problem where method ambiguity would occur if inheriting from two classes with the same method.

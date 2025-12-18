---
{"publish":true,"title":"Abstraction","cssclasses":""}
---

## Definitions

**Abstraction**: Hiding implementation complexity while exposing simple interface. Shows "what" but not "how".

**Abstract Class**: Cannot be instantiated. Contains abstract methods (no implementation) that must be overridden by subclasses.

**Abstract Method**: Method declaration without implementation. Marked with `abstract` keyword; no method body.

**Concrete Method**: Regular method with implementation. Abstract classes can contain concrete methods.

**Partial Implementation**: Abstract classes provide some implementation, leaving flexibility for subclasses to provide rest.

---

## QnA

**Can abstract classes have concrete methods?**

Yes, abstract classes can have both abstract methods (no implementation) and concrete methods (with implementation). This flexibility allows abstract classes to provide common functionality while requiring subclasses to implement specific behavior. For example, a Vehicle abstract class might have concrete method `displayModel()` but abstract methods `start()` and `stop()`.

**Can abstract classes have constructors?**

Yes, constructors are called by subclass constructors via `super()` for initialization. Abstract class constructors initialize parent state. This is crucial for multi-level inheritance where each level properly initializes its fields through the constructor chain.

**Can you instantiate an abstract class?**

No, abstract classes cannot be instantiated. Attempting `new Vehicle()` on an abstract Vehicle class causes a compile error. You must create a concrete subclass implementing all abstract methods, then instantiate the subclass. This enforces the contract defined by the abstract class.

**What's the difference between abstract class and interface?**

Abstract classes support single inheritance, can have instance variables and private members, and provide partial implementation. Interfaces support multiple implementation, originally had all methods abstract (now with default methods), and define contracts. Use abstract classes for "is-a" relationships with shared code; interfaces for capabilities.

**Can abstract methods have a body?**

No, abstract methods are declarations without bodies. If you provide implementation, remove the abstract keyword. The point of abstract methods is to force subclasses to provide their own implementation based on specific behavior needed.

**Must subclasses implement all abstract methods?**

Yes, concrete subclasses must override all abstract methods from parent. If not, the subclass remains abstract and cannot be instantiated. This enforces the contract: all abstract methods must have implementations before the class can be used.

---

## Code Examples

**Example - Abstract Class & Methods:**
```java
abstract class Vehicle {
    protected String model;
    
    public Vehicle(String model) {
        this.model = model;
    }
    
    // Abstract methods - must be implemented by subclasses
    abstract void start();
    abstract void stop();
    abstract void accelerate();
    
    // Concrete method - common to all vehicles
    public void displayModel() {
        System.out.println("Model: " + model);
    }
}

class Car extends Vehicle {
    public Car(String model) {
        super(model);
    }
    
    @Override
    public void start() {
        System.out.println("Car engine starts");
    }
    
    @Override
    public void stop() {
        System.out.println("Car stops");
    }
    
    @Override
    public void accelerate() {
        System.out.println("Car accelerates");
    }
}

// Usage
Vehicle car = new Car("Tesla Model 3");
car.start();
car.accelerate();
car.displayModel();
```

**Example - Partial Implementation:**
```java
abstract class Shape {
    abstract double getArea();
    abstract double getPerimeter();
    
    // Concrete method using abstract methods
    public void printDimensions() {
        System.out.println("Area: " + getArea());
        System.out.println("Perimeter: " + getPerimeter());
    }
}

class Circle extends Shape {
    private double radius;
    
    public Circle(double radius) {
        this.radius = radius;
    }
    
    @Override
    double getArea() {
        return Math.PI * radius * radius;
    }
    
    @Override
    double getPerimeter() {
        return 2 * Math.PI * radius;
    }
}
```

---

## Key Points

- **Contract Definition**: Abstract classes define contract subclasses must follow; concrete implementations vary.
- **Code Reuse**: Abstract classes provide common implementation; subclasses override for specific behavior.
- **Abstraction Level**: Each level of hierarchy can be abstract if providing only partial implementation.
- **Constructor Execution**: Abstract class constructors execute when concrete subclass instantiated.
- **Template Method Pattern**: Abstract class defines algorithm structure with concrete methods; subclasses fill specifics via abstract method overrides.

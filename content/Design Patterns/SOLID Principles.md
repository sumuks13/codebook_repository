tags : [[Design Patterns]]

SOLID is an acronym for five key **object-oriented design principles** that help you write better, cleaner, and more maintainable code.

### 1. **Single Responsibility Principle (SRP)**

> A class should have only one reason to change.

- Each class should do **one thing only**. Don’t put too many responsibilities in a single class.
- Imagine a Restaurant: 
	- A waiter shouldn't be responsible for cooking, cleaning tables and managing the cash register. 
	- Each role (class) should have a clear and distinct responsibility. 
	- In programming, a class like Order should focus on managing order items, not sending confirmations.

### **Example**

```java
// BAD - Multiple responsibilities
class Report {
    public void generateReport() {}
    public void printReport() {}
}
```

```java
// GOOD - Single responsibility
class ReportGenerator {
    public void generateReport() {}
}

class ReportPrinter {
    public void printReport() {}
}
```

### **Advantages**

- Easier to understand
- Easier to test
- Fewer bugs

### 2. **Open/Closed Principle (OCP)**

> Software entities should be **open for extension**, but **closed for modification**.

- You should be able to **add new features** without changing existing code.
- Think of a Toolbox: 
	- You should be able to add new tools (features) without modifying the existing toolbox itself (changing existing code). 
	- This principle encourages designing classes in a way that lets you extend functionalities through inheritance or interfaces, like adding new power tools without redesigning the entire toolbox.

### **Example**

```java
// BAD - Adding new shape requires modifying this class
class AreaCalculator {
    public double calculateArea(Object shape) {
        // logic for shape
        return 0;
    }
}
```

```java
// GOOD - Open for extension using inheritance
interface Shape {
    double area();
}

class Circle implements Shape {
    public double area() {
        return Math.PI * 5 * 5;
    }
}

class Rectangle implements Shape {
    public double area() {
        return 10 * 5;
    }
}

class AreaCalculator {
    public double calculateArea(Shape shape) {
        return shape.area();
    }
}
```

### **Advantages**

- Avoids breaking existing code
- Supports future enhancements easily

### 3. **Liskov Substitution Principle (LSP)**

> Subtypes must be substitutable for their base types.

- If class `B` is a subclass of class `A`, then `A` should be replaceable with `B` without breaking the program.
- Imagine Toy Blocks: 
	- All toy blocks (subtypes) should work seamlessly with regular blocks (base type). 
	- If you replace a rectangular block with a square block, things shouldn't fall apart. 
	- This principle ensures that subclasses behave consistently with their parent classes, like a square block fitting with other blocks even though it has a specific shape.

### **Example**

```java
class Bird {
    void fly() {}
}

class Sparrow extends Bird {
    void fly() {
        System.out.println("Sparrow flying");
    }
}
```

```java
// BAD - Ostrich can't fly but extends Bird
class Ostrich extends Bird {
    void fly() {
        throw new UnsupportedOperationException("Can't fly");
    }
}
```

```java
// GOOD - Split into flyable and non-flyable birds
interface Bird {}

interface FlyingBird extends Bird {
    void fly();
}

class Sparrow2 implements FlyingBird {
    public void fly() {
        System.out.println("Flying");
    }
}

class Ostrich2 implements Bird {
    // no fly method
}
```

### **Advantages**

- Prevents unexpected behavior
- Ensures reliable polymorphism

### 4. **Interface Segregation Principle (ISP)**

> Clients should not be forced to depend on interfaces they do not use.

- Don’t create **big fat interfaces**. Break them into smaller ones.
- Think of a Gym Membership: 
	- You shouldn't have to pay for a full membership with pool access if you only want to use the weight room.
	- This principle promotes creating smaller, more specific interfaces that cater to different client needs. 
	- In programming, a PaymentProcessor interface could be separated into specific interfaces like CreditCardProcessor and CashOnDeliveryProcessor.

### **Example**

```java
// BAD - Interface forces implementation of unused methods
interface Worker {
    void work();
    void eat();
}

class Robot implements Worker {
    public void work() {}
    public void eat() {} // Not needed
}
```

```java
// GOOD - Split interfaces
interface Workable {
    void work();
}

interface Eatable {
    void eat();
}

class Human implements Workable, Eatable {
    public void work() {}
    public void eat() {}
}

class Robot implements Workable {
    public void work() {}
}
```

### **Advantages**

- Cleaner and focused interfaces
- Classes implement only what they need

### 5. **Dependency Inversion Principle (DIP)**

> High-level modules should not depend on low-level modules. Both should depend on abstractions.

- Use **interfaces** instead of concrete classes to reduce tight coupling.
- Imagine a Light Switch: 
	- The switch (high-level module) shouldn't depend on the specific type of light bulb (low-level module) it controls. 
	- This principle encourages relying on abstractions (interfaces) instead of concrete implementations, allowing flexibility in choosing the actual light bulb (implementation) without affecting the switch functionality.

### **Example**

```java
// BAD - Direct dependency on low-level class
class LightBulb {
    public void turnOn() {}
}

class Switch {
    private LightBulb bulb = new LightBulb(); // tightly coupled

    public void operate() {
        bulb.turnOn();
    }
}
```

```java
// GOOD - Depends on abstraction
interface Switchable {
    void turnOn();
}

class LightBulb implements Switchable {
    public void turnOn() {
        System.out.println("Bulb turned on");
    }
}

class Switch {
    private Switchable device;

    public Switch(Switchable device) {
        this.device = device;
    }

    public void operate() {
        device.turnOn();
    }
}
```

### **Advantages**

- Code is flexible
- Easy to swap implementations
- Great for testing (use mocks)


> [!NOTE] References
> https://medium.com/backticks-tildes/the-s-o-l-i-d-principles-in-pictures-b34ce2f1e898

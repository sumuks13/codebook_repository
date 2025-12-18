---
{"publish":true,"title":"Interfaces","cssclasses":""}
---

## Definitions

**Interface**: Contract specifying what methods a class must implement. It is a blueprint of a class. All methods implicitly public; no state or constructors.

**Default Method**: Concrete method in interface (Java 8+) with default implementation. Allows interface evolution without breaking existing implementations.

**Static Method**: Method in interface (Java 8+) belonging to interface, not implementing classes. Not inherited by implementations.

**Functional Interface**: Interface with exactly one abstract method. Can have default/static methods. Target for lambda expressions.

**Marker Interface**: Interface with no methods (e.g., Serializable, Cloneable). Signals capability to JVM/framework.

**Multiple Inheritance**: Java achieves via interfaces. Class can implement multiple interfaces; code achieves interface polymorphism.

---

## QnA

**Can interfaces have constructors?**

No, interfaces have no state and cannot be instantiated. No constructor needed or permitted. Objects are created from implementing classes, which call their own constructors. This distinguishes interfaces from abstract classes that can have constructors.

**What's the default access modifier for interface methods?**

All methods implicitly public. Before Java 9, default methods could hide behind package-private visibility. Java 9 introduced private methods for encapsulation within interface. This ensures implementing classes see consistent public contracts.

**Can a default method be static?**

No, static and default together cause compile error. Static methods belong to interface and aren't inherited; default methods are instance methods inherited. They serve different purposes and cannot combine. Static methods called via interface name; default methods inherited as instance methods.

**What happens if a class implements interfaces with conflicting default methods?**

Compile error if same signature across interfaces. Must override to resolve ambiguity. Use `InterfaceName.super.method()` to call specific interface's default method if needed. This prevents confusion about which default implementation to use.

**Why use interfaces over abstract classes?**

Interfaces support multiple implementation (multiple inheritance of type); abstract classes support single inheritance with state. Use interfaces for behavior contracts; abstract classes for code sharing and state. Interfaces define "what"; abstract classes define "what and how partially."

**Can an interface extend another interface?**

Yes, interfaces can extend multiple interfaces (true multiple inheritance). Methods must not conflict or rename. This creates inheritance hierarchies of contracts. Implementing class must implement all abstract methods from interface hierarchy.

---

## Code Examples

**Example - Basic Interface:**
```java
// Contract defining what implementing classes must do
public interface PaymentProcessor {
    void processPayment(double amount);
    boolean validateCard(String cardNumber);
    void generateReceipt();
}

public class CreditCardProcessor implements PaymentProcessor {
    @Override
    public void processPayment(double amount) {
        System.out.println("Processing credit card: $" + amount);
    }
    
    @Override
    public boolean validateCard(String cardNumber) {
        return cardNumber.length() == 16;
    }
    
    @Override
    public void generateReceipt() {
        System.out.println("Receipt generated");
    }
}

// Polymorphic usage
PaymentProcessor processor = new CreditCardProcessor();
processor.processPayment(99.99);
```

**Example - Default Methods (Java 8+):**
```java
public interface Logger {
    // Concrete method with default implementation
    default void log(String message) {
        System.out.println("[LOG] " + message);
    }
    
    default void logError(String message) {
        System.out.println("[ERROR] " + message);
    }
    
    // Abstract method still required
    void flush();
}

public class ConsoleLogger implements Logger {
    // Can override default
    @Override
    public void log(String message) {
        System.out.println("[CONSOLE] " + message);
    }
    
    @Override
    public void flush() {
        System.out.println("Flushing logs");
    }
}

// Usage
Logger logger = new ConsoleLogger();
logger.log("Test");      // Overridden version
logger.logError("Error"); // Default version
```

**Example - Static Methods in Interface:**
```java
public interface DateUtil {
    // Static method - not inherited
    static String getCurrentDate() {
        return LocalDate.now().toString();
    }
    
    // Default method using static method
    default String getFormattedDate() {
        return "Date: " + DateUtil.getCurrentDate();
    }
}

public class DateHelper implements DateUtil {
}

// Usage
System.out.println(DateUtil.getCurrentDate());  // Via interface

DateHelper helper = new DateHelper();
// helper.getCurrentDate();  // ERROR: static not inherited
System.out.println(helper.getFormattedDate());  // Uses static method
```

**Example - Resolving Diamond Problem:**
```java
interface A {
    default void display() {
        System.out.println("A's display");
    }
}

interface B {
    default void display() {
        System.out.println("B's display");
    }
}

class C implements A, B {
    @Override
    public void display() {
        A.super.display();  // Call A's version
        B.super.display();  // Call B's version
        System.out.println("C's display");
    }
}
```

**Example - Multiple Interface Implementation:**
```java
interface Drawable { void draw(); }
interface Resizable { void resize(int width, int height); }
interface Rotatable { void rotate(int degrees); }

class Shape implements Drawable, Resizable, Rotatable {
    @Override
    public void draw() {
        System.out.println("Drawing shape");
    }
    
    @Override
    public void resize(int width, int height) {
        System.out.println("Resizing");
    }
    
    @Override
    public void rotate(int degrees) {
        System.out.println("Rotating");
    }
}
```

---

## Key Points

- **Interface Inheritance**: Interface extending uses `extends` (multiple allowed). Class implementing uses `implements`.
- **Default Method Pitfalls**: Can complicate inheritance; use judiciously. Prefer small interfaces with single responsibility.
- **Private Methods**: Java 9+ allows private methods in interfaces for code reuse within interface; not inherited.
- **Sealed Interfaces**: Java 17+ can seal interfaces with `permits` to restrict implementations.
- **Marker Interfaces**: Increasingly obsolete; annotations more flexible than marker interfaces.

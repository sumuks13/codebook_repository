---
{"publish":true,"title":"Classes & Objects","cssclasses":""}
---

## Definitions

**Class**: A blueprint or template for creating objects. Defines attributes (state) and methods (behavior).

**Object**: An instance of a class. Contains specific values for the attributes defined in the class.

**Constructor**: A special method invoked when creating an object. Used for initialization; same name as class; no return type.

**Access Specifiers**: Keywords controlling visibility—public (everywhere), protected (subclass/package), default/package-private (same package), private (same class only).

**Method**: A function defined inside a class. Can accept parameters and return values.

**this keyword**: Reference to the current object instance. Used to access instance members and distinguish from local variables.

**Nested Class**: Class defined inside another class. Can be static (not tied to outer) or non-static (inner class).

**Static Nested Class**: Independent class residing inside another class. No implicit outer reference. Can be instantiated without outer instance.

**Inner Class**: Non-static nested class. Has implicit reference to outer class instance. Can access outer class members including private.

**Anonymous Inner Class**: Inline class with no name. Useful for single-use implementations or event handlers.

**Local Inner Class**: Class defined inside method. Scope limited to method. Can access method's effectively final local variables.

---

## QnA

**Can a constructor be private?**

Yes, private constructors prevent instantiation from outside the class. This is used in the singleton pattern to ensure only one instance exists, or in utility classes to prevent instantiation of static method containers. For example, `private MyClass() { }` restricts object creation to the class itself.

**What's the difference between a constructor and a method?**

Constructors initialize objects (no return type, runs once per object creation), while methods perform operations (have return types, called multiple times). Constructors must have the same name as the class; methods can have any name. Constructors run automatically when using `new`; methods require explicit calls.

**Can we overload constructors?**

Yes, constructors can be overloaded with different parameter lists. Overloaded constructors allow multiple ways to initialize an object. For example, a Student class might have `Student(String name)` and `Student(String name, int roll)`. Java matches the constructor based on argument types and count.

**What's the default access modifier?**

If no modifier is specified, members have package-private access (default), meaning they're accessible within the same package but not outside it. This is neither public, protected, nor private. Be explicit with access modifiers in production code; relying on defaults reduces clarity.

**What does the `new` keyword do?**

The `new` keyword performs three operations: allocates memory on the heap for the object, calls the constructor to initialize the object, and returns a reference to the newly created object. For example, `new Student("Alice")` creates a Student object and calls the appropriate constructor.

**Can `this()` and `super()` be called together?**

No, only one can be the first statement in a constructor. Use `this(params)` to call another constructor in the same class or `super(params)` to call the parent constructor. Attempting both causes a compile error. This restriction ensures clear initialization order.

**What's the difference between static and non-static nested classes?**

Static nested classes are independent and don't hold reference to outer instance—can instantiate without outer object. Non-static inner classes require outer instance and can access all outer members including private. Static nested classes are like regular classes; inner classes have implicit `Outer.this` reference, increasing memory footprint.

**Can inner classes access private members of outer?**

Yes, inner classes can access all members (private, protected, public) of the outer class directly. The implicit outer reference grants this access. This enables tight coupling between inner and outer classes, useful for closely related functionality.

**Why use nested classes?**

Nested classes provide logical grouping of related classes, reduce namespace pollution, improve encapsulation, and enable access to outer class private members. Use when class is closely related to outer class; awkward or unusable elsewhere. Improves code organization and intent clarity.

**What's a local inner class?**

A class defined inside a method—scope limited to that method. Can access method's effectively final local variables. Useful for single-use implementations within method scope. Less common than anonymous inner classes but provides more control.

**Can static nested classes access instance members of outer?**

No, static nested classes have no outer instance reference. Can only access static members of outer class directly. Instance members require an outer instance passed explicitly as parameter or created separately.

**What's the memory overhead of inner classes?**

Inner classes hold implicit reference to outer instance (`Outer.this`), using extra memory per instance. Static nested classes avoid this overhead. For many inner instances, this overhead becomes significant. Consider static nested classes for better performance if outer reference not needed.


---

## Code Examples

**Example - Class & Object:**
```java
public class Student {
    // Attributes (instance variables)
    private String name;
    private int rollNumber;
    private double gpa;
    
    // Constructor
    public Student(String name, int rollNumber) {
        this.name = name;           // Using 'this' keyword
        this.rollNumber = rollNumber;
        this.gpa = 0.0;
    }
    
    // Overloaded constructor
    public Student(String name, int rollNumber, double gpa) {
        this.name = name;
        this.rollNumber = rollNumber;
        this.gpa = gpa;
    }
    
    // Methods
    public void displayInfo() {
        System.out.println("Name: " + name + ", Roll: " + rollNumber);
    }
    
    // Getter methods (encapsulation)
    public String getName() { return name; }
    public double getGpa() { return gpa; }
    
    // Setter method
    public void setGpa(double gpa) {
        if (gpa >= 0.0 && gpa <= 4.0) {
            this.gpa = gpa;
        }
    }
}

// Usage
Student student1 = new Student("Alice", 101);
Student student2 = new Student("Bob", 102, 3.8);
student1.setGpa(3.5);
student1.displayInfo();
```

**Example - Static Nested Class:**
```java
public class Outer {
    private static String staticVar = "Static";
    private String instanceVar = "Instance";
    
    // Static nested class - independent
    public static class StaticNested {
        public void display() {
            System.out.println(staticVar);        // Can access static
            // System.out.println(instanceVar);   // ERROR: No instance
        }
    }
}

// Usage - no outer instance needed
Outer.StaticNested nested = new Outer.StaticNested();
nested.display();
```

**Example - Non-Static Inner Class:**
```java
public class Outer {
    private String outerVar = "Outer";
    
    // Non-static inner class - needs outer instance
    public class InnerClass {
        private String innerVar = "Inner";
        
        public void display() {
            System.out.println(outerVar);   // Can access outer
            System.out.println(innerVar);
        }
        
        public void accessOuter() {
            System.out.println(Outer.this.outerVar);  // Explicit
        }
    }
}

// Usage - need outer instance first
Outer outer = new Outer();
Outer.InnerClass inner = outer.new InnerClass();  // Special syntax
inner.display();
```

**Example - Anonymous Inner Class:**
```java
interface Greeting {
    void greet();
}

public class AnonymousExample {
    public static void main(String[] args) {
        // Anonymous inner class implementing interface
        Greeting greeting = new Greeting() {
            @Override
            public void greet() {
                System.out.println("Hello from anonymous");
            }
        };
        greeting.greet();
        
        // Simplified with lambda (Java 8+)
        Greeting lambda = () -> System.out.println("Hello from lambda");
        lambda.greet();
    }
}
```

**Example - Local Inner Class:**
```java
public class LocalInnerExample {
    public void methodWithLocalClass() {
        final int localVar = 10;  // Effectively final
        
        class LocalClass {
            public void display() {
                System.out.println(localVar);  // Accesses local var
            }
        }
        
        LocalClass local = new LocalClass();
        local.display();
    }
}
```

**Example - Inner Class Accessing Private:**
```java
public class OuterWithPrivate {
    private int privateValue = 100;
    
    public class InnerAccess {
        public void accessPrivate() {
            System.out.println("Private: " + privateValue);
        }
    }
}

// Usage
OuterWithPrivate outer = new OuterWithPrivate();
OuterWithPrivate.InnerAccess inner = outer.new InnerAccess();
inner.accessPrivate();  // Prints: Private: 100
```

---

## Key Points

- **Default Constructor**: If no constructor defined, Java provides one that initializes to default values. Explicitly defining constructors removes this automatic one.
- **Constructor Chaining**: Use `this(params)` to call another constructor in same class; reduces code duplication. First line must be this() or super().
- **Access Modifiers in Practice**: Keep data private, provide public getter/setter methods (encapsulation principle). Use protected for subclass access.
- **this vs super**: `this` references current object; `super` references parent class members. Both can access methods and variables.
- **Object Memory**: Each object gets separate instance variables; static members are shared across objects.
- **Memory Overhead**: Inner classes implicitly hold `Outer.this` reference, using extra memory per instance.
- **Instantiation**: Static nested independent; inner classes need outer instance.
- **Scope**: Local inner classes exist only within method scope; cannot be used elsewhere.
- **Serialization**: Anonymous/local inner classes have serialization issues; use serialVersionUID.
- **Modern Alternative**: Lambda expressions (Java 8+) often replace anonymous inner classes for functional interfaces; more concise and readable.
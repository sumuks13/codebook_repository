---
{"publish":true,"title":"Static & Final Keywords","cssclasses":""}
---

## Definitions

**Static Keyword**: Designates class-level members belonging to the class, not individual instances. Shared across all objects.

**Static Variable**: Single copy stored in memory, shared by all instances. Initialized when class loads.

**Static Method**: Called via class name without creating object. Cannot access instance variables or use `this` keyword.

**Static Block**: Initialization block executed once when class loads. Used for static variable initialization.

**Final Keyword**: Modifier preventing modification. 
- On variables = cannot modify (constant); 
- On methods = cannot override; 
- on classes = cannot extend.

---

## QnA

**When should you use the static keyword?**

Use static for class-level functionality that doesn't depend on instance state. Common uses include: shared counters across all instances, utility functions that don't need object state (like Math methods), and factory methods that create instances. For example, a counter tracking total objects created should be static; instance-specific data should not be.

**Can you override static methods?**

No, static methods cannot be overridden because they're resolved at compile-time (static binding) based on the reference type, not the object type. A subclass method with the same signature as a parent static method shadows it rather than overriding. When accessing static methods, use the class name, not an instance.

**Can local variables be declared as static?**

No, the static keyword applies only to class members (variables, methods, nested classes). Local variables inside methods cannot have the static modifier as they don't exist at the class level. Attempting this causes a compile error.

**What's the difference between static and non-static members?**

Static members belong to the class and are shared across all instances, accessed via `ClassName.member`. Non-static members belong to individual instances and accessed via `instanceName.member`. Static members use one memory location; non-static creates separate storage per object. Choose static for shared state; non-static for instance-specific state.

**Can final classes be inherited?**

No, the final keyword on a class prevents subclassing. `public final class MyClass { }` cannot be extended. Java uses this for security (String, Integer classes) or to prevent misuse of immutable implementations. Attempting to extend a final class causes a compile error.

**What if you make a variable final but not initialize it?**

You create a "blank final" that must be initialized before the constructor completes. The compiler enforces this: if a blank final isn't initialized in all code paths before the constructor ends, you get a compile error. This guarantees the field has a value before the object is used.

---

## Code Examples

**Example - Static Variable & Method:**
```java
public class MathUtility {
    // Static variable
    public static final double PI = 3.14159;
    private static int callCount = 0;
    
    // Static method
    public static int add(int a, int b) {
        callCount++;
        return a + b;
    }
    
    public static int getCallCount() {
        return callCount;
    }
    
    // Static block - runs once when class loads
    static {
        System.out.println("MathUtility class loaded");
        callCount = 0;
    }
}

// Usage (no object creation needed)
int result = MathUtility.add(5, 3);
System.out.println(MathUtility.PI);        // 3.14159
System.out.println(MathUtility.getCallCount()); // 1
```

**Example - Final Keyword:**
```java
public class Constants {
    // Final variable = constant, must initialize
    public static final String APP_NAME = "MyApp";
    private final String id;
    
    public Constants(String id) {
        this.id = id;  // Blank final initialization in constructor
    }
    
    // Final method - cannot be overridden
    public final void criticalOperation() {
        System.out.println("This cannot be changed in subclasses");
    }
}

// Final class - cannot be extended
public final class ImmutableClass {
    // Cannot extend: class ImmutableClass extends AnotherClass { }
}
```

---

## Key Points

- **Static Variables**: Allocated in method area, shared across all instances. Initialized to default values before static block runs.
- **Static Context**: Static methods/blocks can only access other static members directly. Instance members require an object reference.
- **Static Binding**: Method calls on static members resolved at compile-time using reference type, not runtime type.
- **Final with Initialization**: Final primitives are constants; final objects' reference is constant, but the object's state can still change.
- **Performance**: Static members accessed directly without object overhead—useful for utility classes. However, overuse reduces flexibility and testability.
- **Memory**: Static variables persist for application lifetime, only garbage collected after class unloads.

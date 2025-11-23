---
{"publish":true,"title":"Java Basics","cssclasses":""}
---

## Definitions

**Data Types**: Classification of information that determines what values a variable can hold and how memory is allocated. Java has 8 primitive types (byte, short, int, long, float, double, char, boolean) and reference types (objects, arrays).

**Variable**: A named container for storing data values. Must be declared with a data type before use in Java's statically-typed system.

**Type Casting**: Converting one data type to another. Implicit casting occurs automatically for compatible types; explicit casting requires the cast operator `(type)`.

**Scope**: The region where a variable is accessible. Java has local scope (method/block), instance scope (object lifetime), and class/static scope (application lifetime).

**Literals**: Fixed values written directly in code (e.g., `10`, `"Hello"`, `3.14`).

**Pass-by-Value**: Java passes copy of variable value to method. For primitives, copy of value; for objects, copy of reference (not copy of object).

**Reference Type**: Variables holding memory address of object, not object itself. Both original and method parameter reference same heap object.

**Reference Copy**: When passing object reference to method, parameter receives copy of the reference, not copy of object.

**Primitive Copy**: When passing primitive to method, parameter receives copy of the value (completely independent).

**Object Mutation**: Changes to object fields visible through any reference (original and parameter); object itself not copied.

---

## QnA

**What is the difference between a variable and a literal?**

A variable is a named storage location that can be modified during execution; a literal is a fixed constant value written directly in code. Variables require declaration with a type, while literals are immediate values that the compiler infers.

**What are the 8 primitive data types in Java?**

The eight primitive data types are: byte, short, int, long, float, double, char, and boolean. Each has a specific range of values and memory footprint. For example, byte ranges from -128 to 127, while long can hold much larger values.

**What are variable scopes in Java?**

Java has three main scopes: **Local** scope (variables declared inside method/block, accessible only within that block), **Instance** scope (variables tied to object lifetime, accessible throughout object methods), and **Static** scope (class-level variables shared across all objects, accessible via class name).

**Can local variables be declared as static?**

No, the static keyword applies only to class members (variables, methods, nested classes). Local variables inside methods cannot have the static modifier as they don't exist at the class level.

**What is autoboxing and unboxing?**

**Autoboxing** is the automatic conversion of primitive types to their wrapper class equivalents (for example, int → Integer). **Unboxing** is the reverse process where wrapper classes are automatically converted to their primitive types. This feature was introduced in Java 5 to simplify code when working with collections that require objects.

**How do you explicitly cast a double to int?**

Use the cast operator before the variable: `int x = (int) 5.5;`. This converts the double value to an integer, resulting in truncation rather than rounding. The fractional part (0.5) is discarded.

**Is Java pass-by-value or pass-by-reference?**

Java is strictly pass-by-value. For primitives, copy of value passed. For objects, copy of reference (address) passed—not copy of object itself. Both original and parameter reference same object, so field modifications visible. Reassignment of parameter doesn't affect original variable's reference.

**Why doesn't reassigning a parameter affect the original variable?**

Parameter receives copy of reference. Reassignment changes local copy, not original variable's reference. For example, if method does `p = new Object()`, only local parameter changes; original variable still references original object. This clarifies the distinction between object modification vs. reference reassignment.

**If I modify object fields in a method, does the original change?**

Yes, both references point to same heap object. Modifying fields affects the shared object. For example, `p.name = "New"` modifies the object both references point to, making change visible to caller. This is pass-by-value applied to references.

**What about passing null?**

Null is passed as copy—method receives null reference. Method can assign new object to null parameter (`p = new Object()`), but original variable remains null. This follows pass-by-value semantics: parameter is local copy.

**Can a method modify array elements?**

Yes, array reference passed as copy, but elements on heap are same. Modifications visible to caller. For example, `arr[0] = 999` affects original array because both reference the same heap array. However, reassigning parameter (`arr = new int[]{...}`) doesn't affect original.

**What's the difference between modifying vs. reassigning?**

**Modify**: Changes object state (fields/elements) through reference—visible to original. **Reassign**: Changes local parameter reference to new object—doesn't affect original. Understanding this distinction is crucial for pass-by-value semantics.

---

## Code Examples

**Example - Variable Declaration & Scope:**
```java
public class ScopeExample {
    static int staticVar = 100;      // Class scope
    int instanceVar = 50;             // Instance scope
    
    void method() {
        int localVar = 20;            // Local scope
        System.out.println(localVar); // Valid
    }
    
    public static void main(String[] args) {
        // System.out.println(localVar);  // ERROR: Not in scope
        System.out.println(staticVar);    // Valid
    }
}
```

**Example - Type Casting:**
```java
int num = 100;
double dbl = num;                    // Implicit (widening)
int back = (int) dbl;                // Explicit (narrowing)
```

**Example - Pass-by-Value with Primitives:**
```java
public class PrimitiveExample {
    public static void modifyValue(int value) {
        value = 100;  // Changes only local copy
    }
    
    public static void main(String[] args) {
        int num = 50;
        modifyValue(num);
        System.out.println(num);  // Still 50
    }
}
// Output: 50
```

**Example - Pass-by-Value with Objects (Modify):**
```java
class Person {
    public String name;
    
    public Person(String name) {
        this.name = name;
    }
}

public class ObjectModification {
    public static void modifyPerson(Person p) {
        p.name = "Bob";  // Modifies heap object
    }
    
    public static void main(String[] args) {
        Person person = new Person("Alice");
        modifyPerson(person);
        System.out.println(person.name);  // Bob (changed!)
    }
}
// Output: Bob
```

**Example - Pass-by-Value with Objects (Reassignment):**
```java
class Person {
    public String name;
    public Person(String name) {
        this.name = name;
    }
}

public class ObjectReassignment {
    public static void reassignPerson(Person p) {
        p = new Person("Charlie");  // Reassigns local reference
    }
    
    public static void main(String[] args) {
        Person person = new Person("Alice");
        reassignPerson(person);
        System.out.println(person.name);  // Alice (not Charlie!)
    }
}
// Output: Alice
```

**Example - Array Pass-by-Value:**
```java
public class ArrayExample {
    public static void modifyArray(int[] arr) {
        arr[0] = 999;           // Modifies heap array
        arr = new int[]{1, 2};  // Reassigns local reference
    }
    
    public static void main(String[] args) {
        int[] original = {10, 20, 30};
        modifyArray(original);
        System.out.println(original[0]);     // 999 (modified!)
        System.out.println(original.length); // 3 (not 2)
    }
}
// Output: 999, 3
```

---

## Key Points

- **Default Values**: Instance variables have defaults (int=0, boolean=false); local variables must be explicitly initialized.
- **Variable Naming**: Must start with letter, underscore, or $; case-sensitive; cannot be Java keywords.
- **Memory Allocation**: Primitives stored on stack; references point to heap objects.
- **Type Safety**: Java enforces type checking at compile-time, preventing ClassCastException in most cases.
- **Wrapper Classes**: Integer, Double, Boolean, etc., allow primitives to be used in collections and as objects.
- **Terminology Precision**: Java strictly pass-by-value; for objects, pass copy of reference, not reference to reference.
- **Reference Copy Implication**: Methods can modify object state but cannot make original variable reference different object.
- **Primitive Independence**: Primitives completely independent; no connection between original and parameter.
- **Arrays as References**: Arrays are objects; reference copied. Element modifications visible; array reassignment not visible.
- **Null Reference**: Passing null valid; method receives null copy and can work with it.
- **Confusion Source**: People conflate "reference semantics" with "pass-by-reference." Java has reference semantics but pass-by-value calling convention.
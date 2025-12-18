---
{"publish":true,"title":"Polymorphism","cssclasses":""}
---

## Definitions

**Polymorphism**: Ability of objects to take multiple forms. Two types: compile-time (method overloading) and runtime (method overriding).

**Method Overloading**: Multiple methods with same name but different parameters in the **same class**. Compile-time (static) polymorphism.

**Method Overriding**: Subclass provides implementation for method from superclass with **same signature**. Runtime (dynamic) polymorphism.

**Dynamic Binding**: Method resolution at runtime based on actual object type, not reference type. Enables polymorphic behavior.

**Static Binding**: Method resolution at compile-time based on reference type. Applied to static methods, private methods, and constructors.

---

## QnA

**What's the difference between overloading and overriding?**

Method overloading occurs in the same class with different parameter lists (count, type, or order)—resolved at compile-time. Method overriding occurs in subclass with the same method signature as parent—resolved at runtime. Overloading provides compile-time flexibility; overriding enables runtime polymorphism. They serve different purposes despite similar names.

**Can you override static methods?**

No, static methods use static binding (compile-time). A subclass method with the same signature as parent's static method hides it (shadowing), not overriding. The method called depends on the reference type (what variable is declared as), not the object type. This is a common source of confusion in interviews.

**Can an overriding method have a different return type?**

Covariant returns are allowed: if parent returns type A, child can return type B if B is a subtype of A. For example, parent returning `Object` and child returning `String` is valid. However, returning an unrelated type causes a compile error.

**What is method hiding?**

Static methods in a subclass "hide" the parent's static method rather than overriding it. The distinction matters: hiding uses compile-time binding (reference type determines which method runs), while overriding uses runtime binding (object type determines execution). This is why you should avoid shadowing static methods.

**Can you have two methods with the same name and different return type?**

No, return type alone doesn't distinguish method overloads. The method signature includes only the method name and parameter list (count, types, order), not the return type. Having different return types with identical names causes a compile error.

**How does dynamic binding work in Java?**

At runtime, the JVM looks up the actual object type (not reference type) to determine which overridden method to call. Reference type is checked at compile-time for type safety. For example, `Animal animal = new Dog(); animal.sound();` calls Dog's sound() at runtime even though referenced as Animal.

---

## Code Examples

**Example - Method Overloading (Compile-time Polymorphism):**
```java
class Calculator {
    // Overload by parameter type
    public int add(int a, int b) {
        return a + b;
    }
    
    public double add(double a, double b) {
        return a + b;
    }
    
    // Overload by parameter count
    public int add(int a, int b, int c) {
        return a + b + c;
    }
    
    // Overload by parameter order
    public void process(int num, String str) {
        System.out.println("int then String");
    }
    
    public void process(String str, int num) {
        System.out.println("String then int");
    }
}

// Usage - compiler chooses method based on argument types
Calculator calc = new Calculator();
System.out.println(calc.add(5, 3));           // int version: 8
System.out.println(calc.add(5.5, 3.2));       // double version: 8.7
System.out.println(calc.add(5, 3, 2));        // three-param version: 10
calc.process(10, "Hello");                     // first version
calc.process("Hello", 10);                     // second version
```

**Example - Method Overriding (Runtime Polymorphism):**
```java
class Animal {
    public void sound() {
        System.out.println("Animal sound");
    }
    
    public void move() {
        System.out.println("Animal moves");
    }
}

class Dog extends Animal {
    @Override
    public void sound() {
        System.out.println("Dog barks");
    }
    // move() not overridden - inherited
}

class Cat extends Animal {
    @Override
    public void sound() {
        System.out.println("Cat meows");
    }
}

// Usage - demonstrates runtime polymorphism
Animal animal1 = new Dog();
Animal animal2 = new Cat();
Animal animal3 = new Animal();

animal1.sound();  // Calls Dog's sound()
animal2.sound();  // Calls Cat's sound()
animal3.sound();  // Calls Animal's sound()
```

**Example - Dynamic Binding in Action:**
```java
class Parent {
    public void display() {
        System.out.println("Parent display");
    }
    
    public static void staticMethod() {
        System.out.println("Parent static");
    }
}

class Child extends Parent {
    @Override
    public void display() {
        System.out.println("Child display");
    }
    
    // Shadows parent's static method
    public static void staticMethod() {
        System.out.println("Child static");
    }
}

// Usage
Parent parent = new Child();
parent.display();           // Runtime: Calls Child's display()
Parent.staticMethod();      // Compile-time: Calls Parent's static
```

---

## Key Points

- **Overloading Resolution**: Compiler chooses method based on reference type and parameter types at compile-time.
- **Overriding Resolution**: JVM looks up actual object type to determine method execution at runtime.
- **Overloading Rules**: Different parameter lists, same or different return types okay, can throw different exceptions.
- **Overriding Rules**: Same signature, same or covariant return type, cannot narrow access modifier, cannot throw broader exceptions.
- **Performance**: Overloading has no runtime cost; overriding has slight cost due to virtual method invocation.

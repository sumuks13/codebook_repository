tags: [[Java/OOPS]]

### 1. What is Runtime Polymorphism?

Runtime Polymorphism or Dynamic Polymorphism is the polymorphism that exists at runtime. In case of method overriding it is not known which method will be called at runtime. Based on the type of object, JVM decides the exact method that should be called.

### 2. Is it possible to achieve Runtime Polymorphism by data members in Java? 

No. We need to create Runtime Polymorphism by implementing methods at two levels of inheritance in Java

### 3. Explain the difference between static and dynamic binding

In Static binding references are resolved at compile time. 
In Dynamic binding references are resolved at Run time. 

E.g.
```java
Person p = new Person(); 
p.walk(); // Java compiler resolves this binding at compile time. 

public void walk(Object o){ 
	((Person) o).walk(); // dynamic binding. obj is converted to at runtime and then walk method is invoked.
}
```
### **What is polymorphism in Java?**

Polymorphism means "**many forms**". It allows a single action or method to behave **differently based on the object** that invokes it.

### **What are the types of polymorphism in Java?**

1. **Compile-time polymorphism** (also called **static binding** or **method overloading**)
2. **Runtime polymorphism** (also called **dynamic binding** or **method overriding**)    

### **What is compile-time polymorphism or method overloading?**

This is achieved using **method overloading**, where multiple methods have the **same name but different parameters**. The method to call is decided at **compile time** based on arguments.

```java
public void add(int a, int b) { }
public void add(double a, double b) { }
```

### **Can method overloading be done by the changing return type?**

No. Method overloading **must differ by parameter list**. Changing return type causes a **compile-time error**.

### **What is runtime polymorphism or method overriding?**

Method overriding is when a subclass **redefines a method** from its superclass with the **same name, return type, and parameters**.

The method to be executed is determined at **runtime**.

```java
class Animal {
    void sound() { System.out.println("Animal sound"); }
}

class Dog extends Animal {
    void sound() { System.out.println("Bark"); }
}
```

### **How does Java achieve runtime polymorphism?**

Through **method overriding** and using a **parent reference** to refer to a **child object**.

```java
Animal a = new Dog();
a.sound(); // Outputs: Bark
```

### **What is dynamic method dispatch?**

It is the process by which **a call to an overridden method** is resolved at runtime using the **object's actual type**, not the reference type.

### **What is the role of the `@Override` annotation?**

Used to tell the compiler that a method is **intended to override** a superclass method.  
It helps catch errors if the method signature doesn't match.

### **What are some key rules for method overriding?**

- Method name, parameters, and return type must be the same
- Cannot override `final` or `static` methods
- Access modifier in the child class must be **equal or more visible**
- Throws clause must be same or narrower

### **Can constructors be overridden?**

No. Constructors are **not inherited**, so they **cannot be overridden**.

### **Can static methods be overridden?**

No. Static methods are **bound at compile time**.  
You can declare the same static method in the subclass, but this is called **method hiding**, not overriding.

### **What is method hiding?**

When a subclass defines a **static method** with the same name and signature as a static method in the parent class.

```java
class A {
    static void test() { }
}

class B extends A {
    static void test() { } // method hiding
}
```

### **Can you achieve polymorphism using interfaces?**

Yes. Polymorphism can be achieved using **interfaces** — a reference of an interface can point to any of its **implementing classes**.

```java
interface Shape {
    void draw();
}

class Circle implements Shape {
    public void draw() { System.out.println("Circle"); }
}

Shape s = new Circle();
s.draw(); //Output: Circle
```

### **Why is polymorphism useful in Java?**

- Improves **code reusability**
- Supports **loose coupling**
- Makes the code **more flexible and scalable**
- Helps follow **OOP principles** like **Open/Closed Principle**

### **What is the difference between overloading and overriding?**

| Feature     | Overloading  | Overriding                     |
| ----------- | ------------ | ------------------------------ |
| Timing      | Compile-time | Runtime                        |
| Scope       | Same class   | Across superclass and subclass |
| Parameters  | Must differ  | Must be exactly same           |
| Return type | Can differ   | Must be same or covariant      |

### **Can you override a private method?**

No. `private` methods are **not inherited**, so they **cannot be overridden**.  
You can declare a method with the same name in the subclass, but it will be a **new method**, not an override.

### **Can polymorphism happen with data members (fields)?**

No. Fields are resolved at **compile-time**, based on the **reference type**, not the object type.

### **What is covariant return type in Java?**

If a method in a subclass **overrides** a method in the superclass, the return type can be a **subtype of the return type** declared in the superclass.

```java
class A {
    Object get() { return null; }
}

class B extends A {
    String get() { return "Hello"; }
}
```

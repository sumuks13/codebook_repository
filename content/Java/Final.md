tags : [[Java/Java]]

### **What is the `final` keyword in Java?**

The `final` keyword is used to **declare constants or prevent changes**. It can be applied to:
1. **variable** -> stop value change
2. **method** -> stop overriding to prevent subclasses from **changing the behavior** of critical methods (e.g., utility or security logic).
3. **class** -> stop inheritance    

### **What does a `final` variable mean?**

A `final` variable is a **constant**. Once it is assigned a value, it **cannot be changed**.

```java
final int MAX = 100;
```

Attempting to reassign will cause a compile-time error.

### **What is a `final` method in Java?**

A method declared `final` **cannot be overridden** by subclasses.

```java
class A {
    final void display() { }
}

class B extends A {
    // void display() {} ❌ — Not allowed
}
```

### **What is a `final` class in Java?**

A `final` class **cannot be subclassed**. Example: `java.lang.String` is a final class.

```java
final class Shape { }

class Circle extends Shape { } // ❌ Error
```

### **Can a `final` variable be uninitialized?**

Yes, but only if it’s:

- A **blank final** (can only be assigned in constructor/initializer)
- A **static final** (can only be assigned in static block)

```java
class PersonA{
	final int age;
    
    public Person() {
        age = 25; // Allowed — assigned once inside constructor
    }
}

public class PersonB{
    final int age;
    
    {
        age = 25; // Allowed — assigned once in initializer block
    }
}
```

```java
static final int age;
static{
	age = 25; //allowed only once
}
```

### **Can a `final` variable refer to a mutable object?**

Yes. A `final` reference means you **cannot change the reference**, but you **can modify the object** it points to.

```java
final List<String> list = new ArrayList<>();
list.add("Java"); // allowed

list = new ArrayList<>(); // ❌ not allowed
```

### **What is the difference between `final`, `finally`, and `finalize()`?**

| Term         | Meaning                                                     |
| ------------ | ----------------------------------------------------------- |
| `final`      | Prevents modification (variable, method, class)             |
| `finally`    | Block that always executes in a `try-catch`                 |
| `finalize()` | Method called by GC before object is destroyed (deprecated) |

### **Can a `final` method be static?**

Yes. A method can be both `static` and `final`.

```java
public static final void show() { }
```

### **What is the use of `static final` variables?**

They are used to create **constants shared across all instances**.

```java
public static final double PI = 3.14;
```

### **What happens if you try to override a `final` method?**

The compiler throws an error:

> _"Cannot override the final method from superclass."_

### **Why should `final` be used for constants?**

It ensures **read-only behavior**, improves **code safety**, and helps the compiler **optimize performance**.

### **Can a `final` class implement an interface?**

Yes. A final class can implement an interface but **cannot be extended**.

```java
final class MyService implements Runnable {
    public void run() { }
}
```

### **Can constructors be final?**

No. Constructors **cannot be final** because they are **never inherited or overridden**.

### **Can an abstract class or method be `final`?**

No. Abstract means "must be overridden", while final means "cannot be overridden" — so they are **opposites**.

### **Can a method be final and synchronized?**

Yes. A method can be both `final` and `synchronized`.

```java
public final synchronized void process() { }
```

### **What are the benefits of using `final` in Java?**

- Ensures **immutability**
- Prevents **accidental inheritance or overrides**
- Helps compiler with **optimizations**
- Improves **thread safety** when used with immutable objects

### **How does the `final` keyword affect performance?**

In some cases, the **JVM can optimize final methods and variables** better because it knows they won't change.

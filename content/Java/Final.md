tags : [[Java]]

The **final keyword** in java is used to restrict the user. The java final keyword can be used in many context. Final can be:

1. variable -> stop value change
2. method -> stop overriding
3. class -> stop inheritance

If you want to create a variable that is initialized at the time of creating object and once initialized may not be changed, it is useful. For example PAN CARD number of an employee. If you try to modify, you get Output: Compile Time Error

#### 1. Can we initialize blank final variable?

Yes, but only in constructor.

```java
class Bike10{  
	final int speedlimit;//blank final variable  
	
	Bike10(){
		speedlimit=70;
		System.out.println(speedlimit);  
	}
}
```

#### Static blank final variable

A static final variable that is not initialized at the time of declaration is known as static blank final variable. It can be initialized only in static block.

#### Example of static blank final variable


```java
class A{  
	static final int data;//static blank final variable  
	static{ 
		data=50;
	}  
   public static void main(String args[]){  
     System.out.println(A.data);  
	}  
}
```


### **What is the `final` keyword in Java?**

The `final` keyword is used to **declare constants or prevent changes**. It can be applied to:

- Variables
    
- Methods
    
- Classes
    

---

### **What does a `final` variable mean?**

A `final` variable is a **constant**. Once it is assigned a value, it **cannot be changed**.

```java
final int MAX = 100;
```

Attempting to reassign will cause a compile-time error.

---

### **Can a `final` variable be uninitialized?**

Yes, but only if it’s:

- A **blank final** (assigned in constructor)
    
- A **static final** (assigned in static block)
    

```java
final int age;
age = 25; // allowed if done once
```

---

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

---

### **Why would you use a `final` method?**

To prevent subclasses from **changing the behavior** of critical methods (e.g., utility or security logic).

---

### **What is a `final` class in Java?**

A `final` class **cannot be subclassed**.

```java
final class Shape { }

class Circle extends Shape { } // ❌ Error
```

Example: `java.lang.String` is a final class.

---

### **Can a `final` variable refer to a mutable object?**

Yes. A `final` reference means you **cannot change the reference**, but you **can modify the object** it points to.

```java
final List<String> list = new ArrayList<>();
list.add("Java"); // allowed
list = new ArrayList<>(); // ❌ not allowed
```

---

### **What is the difference between `final`, `finally`, and `finalize()`?**

|Term|Meaning|
|---|---|
|`final`|Prevents modification (variable, method, class)|
|`finally`|Block that always executes in a `try-catch`|
|`finalize()`|Method called by GC before object is destroyed (deprecated)|

---

### **Can a `final` method be static?**

Yes. A method can be both `static` and `final`.

```java
public static final void show() { }
```

---

### **Can a `final` variable be initialized inside a constructor?**

Yes. A **blank final** instance variable must be initialized in every constructor.

```java
final int x;
MyClass() {
    x = 5;
}
```

---

### **What is the use of `static final` variables?**

They are used to create **constants shared across all instances**.

```java
public static final double PI = 3.14;
```

---

### **What happens if you try to override a `final` method?**

The compiler throws an error:

> _"Cannot override the final method from superclass."_

---

### **Why should `final` be used for constants?**

It ensures **read-only behavior**, improves **code safety**, and helps the compiler **optimize performance**.

---

### **Can a `final` class implement an interface?**

Yes. A final class can implement an interface but **cannot be extended**.

```java
final class MyService implements Runnable {
    public void run() { }
}
```

---

### **Can constructors be final?**

No. Constructors **cannot be final** because they are **never inherited or overridden**.

---

### **Can an abstract class or method be `final`?**

No. Abstract means "must be overridden", while final means "cannot be overridden" — so they are **opposites**.

---

### **Can a method be final and synchronized?**

Yes. A method can be both `final` and `synchronized`.

```java
public final synchronized void process() { }
```

---

### **What are the benefits of using `final` in Java?**

- Ensures **immutability**
    
- Prevents **accidental inheritance or overrides**
    
- Helps compiler with **optimizations**
    
- Improves **thread safety** when used with immutable objects
    

---

### **How does the `final` keyword affect performance?**

In some cases, the **JVM can optimize final methods and variables** better because it knows they won't change.

#### 1. What is Runtime Polymorphism?

Runtime Polymorphism or Dynamic Polymorphism is the polymorphism that exists at runtime. In case of method overriding it is not known which method will be called at runtime. Based on the type of object, JVM decides the exact method that should be called.

#### 2. Is it possible to achieve Runtime Polymorphism by data members in Java? 

No. We need to create Runtime Polymorphism by implementing methods at two levels of inheritance in Java

#### 3. Explain the difference between static and dynamic binding

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

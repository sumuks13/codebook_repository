tags: [[Design Patterns]], [[Creational Design Patterns]]

### **What is the Factory Pattern?**

- The **Factory Pattern** is a **creational design pattern** that provides an interface for creating objects but **lets subclasses decide** which class to instantiate.
- The Factory pattern creates objects without exposing the creation logic. There will be a common interface through which the newly created object is referenced - [[Java/Polymorphism]].

### **When to use it?**

- When the exact type of object isn’t known until runtime.
- When you want to hide the object creation logic.
- When you have **many subclasses** of a class or interface

### **What are the advantages?**

- Promotes **loose coupling** between client and product classes
- Helps in managing complex object creation logic
- Common in **frameworks** where object creation can vary (like Spring Beans)

### **Example**

Step 1: Define an Interface

```java
public interface Notification {
    void notifyUser();
}
```

Step 2: Implement Concrete Classes

```java
public class EmailNotification implements Notification {
    @Override
    public void notifyUser() {
        System.out.println("Sending Email Notification");
    }
}

public class SMSNotification implements Notification {
    @Override
    public void notifyUser() {
        System.out.println("Sending SMS Notification");
    }
}
```

Step 3: Create the Factory Class

```java
public class NotificationFactory {
    public static Notification createNotification(String type) {
        if ("EMAIL".equalsIgnoreCase(type)) {
            return new EmailNotification();
        } else if ("SMS".equalsIgnoreCase(type)) {
            return new SMSNotification();
        }
        throw new IllegalArgumentException("Unknown notification type");
    }
}
```

Step 4: Use the centralized factory method for object creation instead of `new` keyword.

```java
@RestController
public class NotificationController {

    @GetMapping("/notify")
    public String sendNotification(@RequestParam String type) {
        Notification notification = NotificationFactory.createNotification(type);
        notification.notifyUser();
        return "Notification sent!";
    }
}
```

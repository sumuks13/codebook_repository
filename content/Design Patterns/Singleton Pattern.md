tags: [[Design Patterns]], [[Creational Design Patterns]]

### **What is the Singleton Pattern?**

The **Singleton Pattern** ensures that **only one object (instance) of a class exists in the entire application**, and provides a global way to access it.

It is implemented by:
1. **Static Instance Variable:** A static variable holds the single instance of the class. 
2. **Private Constructor:** The constructor of the class is made private to prevent direct instantiation. 
3. **Static Factory Method:** A static method that either creates the instance if it doesn't exist or returns the existing instance.

### **When to use it?**

- You want only **one instance** of a class to exist
- You need a **centralized resource** (e.g., configuration, logging, cache)
- You want **global access** to the same object

### **Example**

```java
public class AppConfig {

    // 1. Static instance variable
    private static AppConfig instance;

    // 2. Private constructor to prevent outside instantiation
    private AppConfig() {}

    // 3. Static factory method to provide access to the single instance
    public static AppConfig getInstance() {
        if (instance == null) {
            instance = new AppConfig();  // Lazy initialization
        }
        return instance;
    }
}
```

```java
public class Main {
    public static void main(String[] args) {
        AppConfig config1 = AppConfig.getInstance();
        AppConfig config2 = AppConfig.getInstance();

        System.out.println(config1 == config2);  // true
    }
}
```


> [!NOTE] Note
> - In Spring, beans are **singleton by default**
> - The factory method must be **thread-safe** (use synchronized block or enum)

```java
public class SafeSingleton {
    private static SafeSingleton instance;

    private SafeSingleton() {}

    public static synchronized SafeSingleton getInstance() {
        if (instance == null) {
            instance = new SafeSingleton();
        }
        return instance;
    }
}
```
